# Handling basic built-in intents
This tutorial will guide you to handle basic intents such as "yes" or "no," using the Sample Dice extension created in [building a basic extension](/Develop/Tutorials/Build_Simple_Extension.md).

Clova has frequently used basic intents predefined as [built-in intents](/Design/Design_Custom_Extension.md#BuiltinIntent), which can be used by all extensions. The built-in intents provided by Clova are as follows:

| Built-in intent       | Intent               | Sample response utterance                                      |
|---------------------------|-------------------|----------------------------------------------------------|
| Clova.GuideIntent         | Help request          | "What can you do?," "Tell me how to use it" |
| Clova.CancelIntent        | Undo action request        | "Cancel," "Cancel please"                                          |
| Clova.YesIntent           | Positive response (e.g. Yes.)   | "Yes," "Sure," "All right," "Certainly," "Okay"                   |
| Clova.NoIntent            | Negative response (e.g. No) | "No," "No, thank you," "Absolutely not"                                     |

To handle a help request or undo an action request from the user, you can use the built-in intents in the above table without registering intents and sample phrases, just like in the first tutorial.

The process of handling built-in intents is as follows:
* Step 1. Implement built-in intent processing (on the extension server)
* Step 2. Test the built-in intent operation (on the Clova developer console)

## Step 1. Implementing built-in intent processing {#Step1}

You must change the code so that the Sample Dice extension can handle built-in intents.

To help you understand the basic handling method, this section of the tutorial will have you set your extension to handle the help request built-in intent only.
The help request built-in intent is sent when user utterances contain phrases such as "How do I use this?" or "I need help." It uses the name, `Clova.GuideIntent`.

In order to handle this built-in intent, we will reuse the repository from the [first tutorial](/Develop/Tutorials/Build_Simple_Extension.md).
Go to the local repository and check out the `tutorial2` branch as shown below:

```bash
cd /path/to/your_sample_dice_directory
git fetch
git checkout tutorial2
```

The Sample Dice extension handles the help request in the `intentRequest()` of the `clova/index.js` file.

```javascript
intentRequest(cekResponse) {
  const intent = this.request.intent.name

  switch (intent) {
    ...
    case "Clova.GuideIntent":
    default:
      cekResponse.setSimpleSpeechText (Try saying "Throw one die.")
  }
  ...
}
```

As shown from the codes above, the intent is extracted from the request message sent from Clova. If the intent name is `Clova.GuideIntent`, it prompts the user to try saying "Throw one die."

Run the new code on the extension server.

## Step 2. Testing the built-in intent operation {#Step2}
You must test whether the Sample Dice extension handles the help request built-in intent from the extension via an appropriate method.

There are two test methods shown in the [first tutorial](/Develop/Tutorials/Build_Simple_Extension.md). One method is checking the operation of the interaction model on the Clova developer console and the other is checking the actual operation of the Clova app by registering a tester ID.
This tutorial checks the operation of the interaction model only.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>Built-in intents work by default even if not specified in the interaction model. We are planning to allow selective registration of built-in intents for extensions in the future.</p>
</div>

Follow the steps below to check whether the help request of the Sample Dice extension is operating properly:

1. Access the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a>.
2. Click **{{ book.DevConsole.cek_edit}}** on the **{{ book.DevConsole.cek_interaction_model }}** item of the Sample Dice extension.
3. Click **{{ book.DevConsole.cek_builder_menu_build }}** on the top-left section of the screen to build the interaction model.
4. Once the build is complete, click the **{{ book.DevConsole.cek_test }}** menu from the menu list on the left.
5. Enter the sentence to request help in **{{ book.DevConsole.cek_builder_test_expression_title }}**. For example, enter "Give me the instructions."
6. Press Enter or click the **{{ book.DevConsole.cek_builder_test_request_test }}** button.
7. Check whether "Clova.GuideIntent" appears in the **{{ book.DevConsole.cek_builder_test_intent_result }}** field under the **{{ book.DevConsole.cek_builder_test_result_title }}**.<br />
	![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Intent_Test.png)
  <div class="note">
  	<p><strong>Note!</strong></p>
  	<p>If you have not registered an extension server URI that can be accessed externally, a "{{ book.DevConsole.cek_builder_test_no_response }}" message is displayed as a <strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>.</p>
	</div>

This way, the Sample Dice extension can respond to help requests.
If you implement the extension server to handle `Clova.CancelIntent`, `Clova.YesIntent`, and `Clova.NoIntent` using this method, the extension will be able to respond to undo actions and positive or negative requests.
