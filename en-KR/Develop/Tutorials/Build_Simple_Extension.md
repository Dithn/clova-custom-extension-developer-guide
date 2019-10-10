# Building a basic extension
In this tutorial, we will create a basic extension that rolls one die upon user request.

The following factors are mandatory in order to service a custom extension:

{% include "/Develop/Guides/RequiredComponents/Interaction_Model.md" %}

{% include "/Develop/Guides/RequiredComponents/Extension_Server.md" %}

Follow the steps below to find out how to create and register the above factors.

The overall process of creating an extension is as follows:
* Step 1. Preparing an extension server (individually)
* Step 2. Registering basic extension information (on the Clova developer console)
* Step 3. Registering an interaction model (on the Clova developer console)
* Step 4. Testing the extension (on the Clova developer console and real device)

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>To deploy the extension created this way as an actual service, see <a href="/DevConsole/Guides/Deploy_Custom_Extension.md">Deploying an extension</a>.</p>
</div>

## Step 1. Preparing an extension server {#Step1}

You must test whether the Sample Dice extension properly handles the registered slot.

There are two test methods shown in the [first tutorial](/Develop/Tutorials/Build_Simple_Extension.md). One method is checking the operation of the interaction model on the Clova developer console and the other is checking the actual operation of the Clova app by registering a tester ID.
This tutorial checks the operation of the interaction model only.

Connect to the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a> and check that the Sample Dice extension correctly recognizes the number of dice by completing the following steps:

1. Click **{{ book.DevConsole.cek_edit}}** on the **{{ book.DevConsole.cek_interaction_model }}** item of the Sample Dice extension.
2. Click **{{ book.DevConsole.cek_builder_menu_build }}** on the top-left section of the screen to build the interaction model.
3. Once the build is complete, select the **{{ book.DevConsole.cek_test }}** menu from the menu list on the left.
4. In **{{ book.DevConsole.cek_builder_test_expression_title }}**, type in a sentence to roll multiple dice. For example, type in "Roll two dice."
5. Press Enter or click the **{{ book.DevConsole.cek_builder_test_request_test }}** button.
6. Check that `ThrowDiceIntent` appears in the **{{ book.DevConsole.cek_builder_test_intent_result }}** field under the **{{ book.DevConsole.cek_builder_test_result_title }}** `diceCount` appears in the **{{ book.DevConsole.cek_builder_test_slot_result }}** field and that the correct number of dice appears in the **{{ book.DevConsole.cek_builder_test_slot_data}}** field.<br />
	![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slot_Test.png)
  <div class="note">
  	<p><strong>Note!</strong></p>
  	<p>If you have not registered an extension server URI that can be accessed externally, a "{{ book.DevConsole.cek_builder_test_no_response }}" message is displayed as a <strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>.</p>
	</div>
7. Repeat steps 4-6 with sentences such as "Roll ten dice" or "Throw four dice."

If the speech recognition is unsatisfactory, you can increase the probability of recognition by adding further types of sample utterances.

### Tips for developing an extension server {#Tip}

Clova analyzes user utterances and sends the analyzed results to the extension server, so the server should be implemented to respond accordingly to the received details.

* **If the analyzed result is the separated registered user intent ([Custom intent](/Design/Design_Custom_Extension.md#CustomIntent)),**

	Clova sends one of three types of request messages: run extension, run registered intent, or exit extension request types. When the server receives these requests, the server must handle the processes and return the results of starting the extension, handling the designated intent, or exiting the extension.

* **If the analyzed result is the intent provided by Clova as the default ([built-in intent](/Design/Design_Custom_Extension.md#BuiltinIntent)),**

	Clova sends a request message such as a help guide or a positive, negative, or undo action accordingly. When the server receives these requests, the server must return a typical response.

## Step 2. Registering the basic extension information {#Step2}

Access the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a> and register the basic information of the extension.
The main items are as follows:

* Extension information
  * **{{ book.DevConsole.cek_id }}**: The unique ID value of the extension. Generally uses a combination of the package name and extension name. Enter the extension ID of Sample Dice as "my.clova.extension.sampledice".
  * **{{ book.DevConsole.cek_invocation_name }}**: The name used to call the extension. Select a word that can be easily recognized by the Clova app or speaker devices. The invocation name used for the Sample Dice extension is "Sample Dice."
* Server connection settings
  * **{{ book.DevConsole.cek_service_endpoint_url }}**: The REST API server of the extension to communicate with Clova. It must be a publicly accessible URI. In Step 1, enter the URL of the server that executed the Sample Dice code.
		<div class="note">
			<p><strong>Note!</strong></p>
			<p>An HTTP connection can be used for testing but an HTTPS connection is required for the official service. The extension server must use ports 80 and 443 for HTTP and HTTPS connections respectively.</p>
		</div>
  * **{{ book.DevConsole.cek_account_linking }}**: Only use when connecting with the membership information of a third-party using an authorization server (based on OAuth 2.0). Set the Sample Dice extension as **{{ book.DevConsole.cek_no }}**.
* Deployment information and privacy and compliance information<br />
  This information is required for extension review and deployment. You do not need to enter this information when following the tutorial.

## Step 3. Registering an interaction model {#Step3}

You can register the interaction model on the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a>.

In this tutorial, the Sample Dice extension rolls one dice by default if the user requests to roll the dice without stating the number of dice. For this tutorial, we will create a simple interaction model to handle the command for throwing a dice. Since the information on the number of dice is not collected, we can register one intent without a slot.

### Creating a new custom intent
Follow the steps below to make a simple intent to roll one dice upon user request.

1. Click **{{ book.DevConsole.cek_edit }}** on the **{{ book.DevConsole.cek_interaction_model }}** item of the Sample Dice extension.
2. Click <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> next to **{{ book.DevConsole.cek_builder_list_title_intent }}**.
3. Enter the name "ThrowDiceIntent" in the input field below **{{ book.DevConsole.cek_builder_new_intent }}**.
4. Press Enter or click **{{ book.DevConsole.cek_builder_new_intent_create }}** next to the input field.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_NewIntent.png)
	<div class="note">
	  <p><strong>Note!</strong></p>
		<p>The intent name is case-sensitive.</p>
	</div>

### Adding a phrase to the sample utterance list
Follow the steps below to configure the user utterances to be processed with the intent created above. We will only add one user utterance for this tutorial, but it is beneficial to have as many sample utterances as possible.

1. Enter "Throw dice" in the **{{ book.DevConsole.cek_builder_intent_expression_title }}** field.
2. Press Enter or click the <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> button.
3. Once you have finished entering all sample utterances, click **{{ book.DevConsole.cek_save }}**.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_SpeechExample.png)

### Building and testing
To check that the interaction model is operating as intended, build and test the interaction model.

1. Click **{{ book.DevConsole.cek_builder_menu_build }}** in the upper-left corner of the **Custom extension** screen.
	<div class="note">
	  <p><strong>Note!</strong></p>
		<p>The build takes around 3-5 minutes to complete. Once the build starts, the button changes to a <strong>{{ book.DevConsole.cek_builder_menu_build_in_progress }}</strong> button. It returns to a <strong>{{ book.DevConsole.cek_builder_menu_build }}</strong> button again once the build is complete.</p>
	</div>
2. Once the build is complete, click the **{{ book.DevConsole.cek_test }}** menu below the **{{ book.DevConsole.cek_builder_menu_build }}** button.
3. Enter the sentence to test in the **{{ book.DevConsole.cek_builder_test_expression_title }}** field. For example, enter "Roll the dice"
4. Press Enter or click the **{{ book.DevConsole.cek_builder_test_request_test }}** button.
5. Check whether "ThrowDiceIntent" appears in the **{{ book.DevConsole.cek_builder_test_intent_result }}** field under the **{{ book.DevConsole.cek_builder_test_result_title }}**.<br />
  ![](/Develop/Assets/Images/CEK_Tutorial_Test.png)
	<div class="note">
  	<p><strong>Note!</strong></p>
  	<p>If you have not registered an extension server URI that can be accessed externally in step 2, <strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong> is displayed as "{{ book.DevConsole.cek_builder_test_no_response }}".</p>
	</div>

## Step 4. Testing the actual execution of the extension {#Step4}

Once you have checked that the interaction model is working properly, you must check that speech recognition and responses are operating as intended on an actual device before requesting a review.

### Registering a tester ID

Register a tester ID to specify an account to execute the extension.

1. Access the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a>.
2. Click **{{ book.DevConsole.cek_edit }}** on the **{{ book.DevConsole.cek_skill_info }}** item of the Sample Dice extension.
3. Find **{{ book.DevConsole.cek_tester }}** on the displayed screen and enter your {{ book.ServiceEnv.OrientedService }} account ID.
4. Click **{{ book.DevConsole.cek_save }}**.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Wait a short while after registering the tester ID to test the extension. If you are unable to test the extension after about an hour, make an inquiry through the forum or contact the Clova partnership team.</p>
</div>

<div class="note">
	<p><strong>Note!</strong></p>
  <p>To test on an actual device, you must register an externally accessible actual extension server URL in <strong>{{ book.DevConsole.cek_skill_info }}</strong>.</p></li>
</div>

### Running from the Clova app

Run the Sample Dice extension using the Clova app.

1. Install the Clova app on the testing device.
2. Log in using the {{ book.ServiceEnv.OrientedService }} account registered as your tester ID.
3. Make a voice command using the invocation name of the tested extension. For example, say "Clova, ask Sample Dice to roll a die."
4. Check whether the Clova app responds with "Throwing 1 die."

The extension is ready for service once it is confirmed to operate correctly on an actual device. You can now request a review on the Clova developer console and deploy the extension.
