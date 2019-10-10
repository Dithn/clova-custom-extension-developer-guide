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

1. Click **{{ book.DevConsole.cek_edit }}** on the **{{ book.DevConsole.cek_interaction_model }}** item of the Sample Dice extension.
2. Click <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> next to **{{ book.DevConsole.cek_builder_list_title_slottype }}**.
3. Select the `CLOVA.NUMBER` checkbox in the table below **{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}**.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Register_Slot_Type.png)
4. Click **{{ book.DevConsole.cek_save }}** next to **{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}**.

### Registering slots to an intent
The number of dice to roll is additional information required for the action of rolling dice. Since we registered the action of rolling the dice as an intent named `ThrowDiceIntent` in the first tutorial, we now need to register a slot to this intent to indicate the number of dice.
You can register the slot as follows on the same window that was previously used for declaring the slot type:

1. Select `ThrowDiceIntent`, a custom intent, below **{{ book.DevConsole.cek_builder_list_title_intent }}**.
2. Type in "diceCount" in the input field below **{{ book.DevConsole.cek_builder_intent_slot_title }}**.
3. Press Enter or click the <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> button next to the input field.
4. Click the **{{ book.DevConsole.cek_builder_utterance_select_slot }}** combo box next to the registered "diceCount."
5. On the displayed list, select `CLOVA.NUMBER` of the **{{ book.DevConsole.cek_builder_select_slottype_builtin }}** registered earlier.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Add_Slot.png)
6. Click **{{ book.DevConsole.cek_save }}** in the top-right corner of the screen.

### Entering a sample utterance
In the first tutorial, we added a simple sample utterance for throwing a die. Now, we must enter a new sample utterance to specify the number of dice to roll using a slot.

On the same window used to register the slot, enter the sample utterance as follows:

1. Type in "Roll two dice" in the input field below **{{ book.DevConsole.cek_builder_intent_expression_title }}**.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Sample_Utterance.png)
2. Press Enter or click the <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> button.
3. Drag-and-select the text "Two" with the mouse from the list of registered sentences.
4. Select "diceCount" below **{{ book.DevConsole.cek_builder_slot_layer_select_slot }}**.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Set_Slot.png)
5. Repeat steps 1-4 with sentences "Throw one" and "Roll five dices."

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

1. Click **{{ book.DevConsole.cek_edit}}** on the **{{ book.DevConsole.cek_interaction_model }}** item of the Sample Dice extension.
2. Click **{{ book.DevConsole.cek_builder_menu_build }}** in the top-left section of the screen to build the interaction model.
3. Once the build is complete, select the **{{ book.DevConsole.cek_test }}** menu from the menu list on the left.
4. In **{{ book.DevConsole.cek_builder_test_expression_title }}**, type in a sentence to roll multiple dice. For example, type in "Roll two dice."
5. Press Enter or click **{{ book.DevConsole.cek_builder_test_request_test }}**.
6. Check that `ThrowDiceIntent` appears in the **{{ book.DevConsole.cek_builder_test_intent_result }}** field under the **{{ book.DevConsole.cek_builder_test_result_title }}** `diceCount` appears in the **{{ book.DevConsole.cek_builder_test_slot_result }}** field and that the correct number of dice appears in the **{{ book.DevConsole.cek_builder_test_slot_data}}** field.<br />
  <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slot_Test.png" />
  <div class="note">
  	<p><strong>Note!</strong></p>
  	<p>If you have not registered an extension server URI that can be accessed externally, a "{{ book.DevConsole.cek_builder_test_no_response }}" message is displayed as a <strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>.</p>
	</div>
7. Repeat steps 4-6 with sentences, such as "Roll ten dice" or "Throw four dice."

If the speech recognition is unsatisfactory, you can increase the probability of recognition by adding further types of sample utterances.

Now your Sample Dice extension can throw more than one die.
Using the same method, you can also have an extension throw a die multiple times or throw a non-numeric dice.
