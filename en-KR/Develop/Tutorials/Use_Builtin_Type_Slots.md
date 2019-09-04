# Utilizing user-input information
This tutorial will guide you to make the Sample Dice extension created in [Building a basic extension](/Develop/Tutorials/Build_Simple_Extension.md) for rolling more than one die depending on the user request.

Voice commands of users can contain the action that needs to be performed by an extension and also additional information for the action. But first, let us recap on how to use the Sample Dice extension as described in [Tutorial introduction](/Develop/Tutorials/Introduction.md).

{% include "/Develop/Tutorials/BasicInformation/DICE_Sample_Dialog.md" %}

In the second dialogue, the user requested "Throw **2** dice." Here, "2" is the additional information required for an action called "throwing dice".

In an interaction, such additional information is called a [slot](/Design/Design_Custom_Extension.md#Slot). For the extension to use the additional information, you must first identify the type of additional information that could be used and register corresponding slots when defining the interaction model. Clova can use these registered slots to recognize additional information in user requests.

The methods for handling additional information are as follows:
* Step 1. Registering slots to the interaction model (on the Clova developer console)
* Step 2. Implementing slot processing (on the extension server)
* Step 3. Testing the slot operation (on the Clova developer console)

## Step 1. Adding a slot to the interaction model {#Step1}

You must modify the interaction model so that users can specify the number of dice to roll.

To do so, you must first decide on the type of slot information and declare the slot type in the interaction model. Then, add these slots to the relevant intent and help Clova recognize the slots by inputting user sample utterances that contain additional information.

### Declaring slot types to use
Depending on the type of additional information the slot is to hold, you must decide on the slot type. For example, the information type of the number of dice is number.

Commonly used information types are predefined in Clova so that it can be universally used by the extension, and this is called the [built-in slot type](/Design/Design_Custom_Extension.md#BuiltinSlotType). You do not need to create a new slot type if it is already defined in the built-in slot type. For example, there is no need for you to create an information type for the number of dice as it is predefined as a `CLOVA.NUMBER` built-in slot type.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>You can handle the information types unique to extension, which are not defined as built-in slot types, by defining them as <a href="/Design/Design_Custom_Extension.md#CustomSlotType">custom slot types</a>.</p>
</div>

Connect to the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a> and declare the slot type to use in the Sample Dice extension as follows:

<ol>
  <li><p>Click <strong>{{ book.DevConsole.cek_edit }}</strong> on the <strong>{{ book.DevConsole.cek_interaction_model }}</strong> item of the Sample Dice extension.</p></li>
  <li><p>Click <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> next to <strong>{{ book.DevConsole.cek_builder_list_title_slottype }}</strong>.</p></li>
  <li>
    <p>Select the <code>CLOVA.NUMBER</code> checkbox in the table below <strong>{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}</strong>.</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Register_Slot_Type.png" />
  </li>
  <li><p>Click <strong>{{ book.DevConsole.cek_save }}</strong> next to <strong>{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}</strong>.</p></li>
</ol>

### Registering slots to an intent
The number of dice to roll is additional information required for the action of rolling dice. Since we registered the action of rolling the dice as an intent named `ThrowDiceIntent` in the first tutorial, we now need to register a slot to this intent to indicate the number of dice.
You can register the slot as follows on the same window that was previously used for declaring the slot type:

<ol>
  <li><p>Select <code>ThrowDiceIntent</code>, a custom intent, below <strong>{{ book.DevConsole.cek_builder_list_title_intent }}</strong>.</p></li>
  <li><p>Enter "diceCount" in the input field below <strong>{{ book.DevConsole.cek_builder_intent_slot_title }}</strong>.</p></li>
  <li><p>Press Enter or click the <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> button next to the input field.</p></li>
  <li><p>Click the <strong>{{ book.DevConsole.cek_builder_utterance_select_slot }}</strong> combo box next to the registered "diceCount."</p></li>
  <li>
    <p>On the displayed list, select <code>CLOVA.NUMBER</code> of the <strong>{{ book.DevConsole.cek_builder_select_slottype_builtin }}</strong> registered earlier.</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Add_Slot.png" />
  </li>
  <li><p>Click <strong>{{ book.DevConsole.cek_save }}</strong> on the top-right corner of the screen.</p></li>
</ol>

### Entering a sample utterance
In the first tutorial, we added a simple sample utterance for throwing a die. Now, we must enter a new sample utterance to specify the number of dice to roll using a slot.

On the same window used to register the slot, enter the sample utterance as follows:

<ol>
  <li>
    <p>Enter "Roll two dice" in the input field below <strong>{{ book.DevConsole.cek_builder_intent_expression_title }}</strong>.</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Sample_Utterance.png" />
  </li>
  <li><p>Press Enter or click the <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> button.</p></li>
  <li><p>Drag-and-select the text "Two" with the mouse from the list of registered sentences.</p></li>
  <li>
    <p>Select "diceCount" below <strong>{{ book.DevConsole.cek_builder_slot_layer_select_slot }}</strong>.</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Set_Slot.png" />
  </li>
  <li><p>Repeat steps 1-4 with sentences "Throw one" and "Roll five dices."</p></li>
</ol>

## Step 2. Implementing slot processing {#Step2}

You must change the code so that the Sample Dice extension can handle the slot.
In this section, we will reuse the repository from the [first tutorial](/Develop/Tutorials/Build_Simple_Extension.md) in order to see which parts were changed by referring to the source codes implemented before.
Go to the local repository and check out the `tutorial3` branch as shown below:

```bash
cd /path/to/your_sample_dice_directory
git fetch
git checkout tutorial3
```

The Sample Dice extension handles the slot in `intentRequest()` of the `clova/index.js` file.

```javascript
intentRequest(cekResponse) {
  const intent = this.request.intent.name
  const slots = this.request.intent.slots

  switch (intent) {
    case "ThrowDiceIntent":
      let diceCount = 1
      if (!!slots) {
        const diceCountSlot = slots.diceCount
        if (slots.length != 0 && diceCountSlot) {
          diceCount = parseInt(diceCountSlot.value)
        }

        if (isNaN(diceCount)) {
          diceCount = 1
        }
      }
      ...
  }
  ...
}
```

As shown by the codes above, the extension extracts slot information (`this.request.intent.slots`) from the request message sent by Clova. If the "diceCount" slot (`slots.diceCount`) registered in the interaction model exists, the extension reads the slot value as an integer. This value is the number of dice to roll. The extension determines the value as 1 by default unless the slot does not exist or the value is not an integer.

Run the new code on the extension server.

## Step 3. Testing the slot action {#Step3}

You must test whether the Sample Dice extension properly handles the registered slot.

There are two test methods shown in the [first tutorial](/Develop/Tutorials/Build_Simple_Extension.md). One method is checking the operation of the interaction model on the Clova developer console and the other is checking the actual operation of the Clova app by registering a tester ID.
This tutorial checks the operation of the interaction model only.

Connect to the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a> and check that the Sample Dice extension correctly recognizes the number of dice by completing the following steps:

<ol>
  <li><p>Click <strong>{{ book.DevConsole.cek_edit}}</strong> on the <strong>{{ book.DevConsole.cek_interaction_model }}</strong> item of the Sample Dice extension.</p></li>
  <li><p>Click <strong>{{ book.DevConsole.cek_builder_menu_build }}</strong> on the top left section of the screen to build the interaction model.</p></li>
  <li><p>Once the build is complete, select the <strong>{{ book.DevConsole.cek_test }}</strong>  menu from the menu list on the left.</p></li>
  <li><p>In the <strong>{{ book.DevConsole.cek_builder_test_expression_title }}</strong>, enter a sentence to roll multiple dice. For example, enter "Roll two dice."</p></li>
  <li><p>Press Enter or click the <strong>{{ book.DevConsole.cek_builder_test_request_test }}</strong> button.</p></li>
  <li>
    <p>Check that <code>ThrowDiceIntent</code> appears in the <strong>{{ book.DevConsole.cek_builder_test_intent_result }}</strong> field under the <strong>{{ book.DevConsole.cek_builder_test_result_title }}</strong>, <code>diceCount</code> appears in the <strong>{{ book.DevConsole.cek_builder_test_slot_result }}</strong> field, and that the correct number of dice appears in the <strong>{{ book.DevConsole.cek_builder_test_slot_data}}</strong> field.</p>
  	<img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slot_Test.png" />
    <div class="note">
    	<p><strong>Note!</strong></p>
    	<p>If you have not registered an extension server URI that can be accessed externally, a "{{ book.DevConsole.cek_builder_test_no_response }}" message is displayed as a <strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>.</p>
  	</div>
  </li>
  <li><p>Repeat steps 4-6 with sentences such as "Roll ten dice" or "Throw four dice."</p></li>
</ol>

If the speech recognition is unsatisfactory, you can increase the probability of recognition by adding further types of sample utterances.

Now your Sample Dice extension can throw more than one die.
Using the same method, you can also have an extension throw a die multiple times or throw a non-numeric dice.
