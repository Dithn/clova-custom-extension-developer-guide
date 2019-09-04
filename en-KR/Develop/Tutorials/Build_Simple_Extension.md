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

<ol>
  <li><p>Click <strong>{{ book.DevConsole.cek_edit}}</strong> on the <strong>{{ book.DevConsole.cek_interaction_model }}</strong> item of the Sample Dice extension.</p></li>
  <li><p>Click <strong>{{ book.DevConsole.cek_builder_menu_build }}</strong> on the top left section of the screen to build the interaction model.</p></li>
  <li><p>Once the build is complete, select the <strong>{{ book.DevConsole.cek_test }}</strong> menu from the menu list on the left.</p></li>
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

### Tips for developing an extension server {#Tip}

Clova analyzes user utterances and sends the analyzed results to the extension server, so the server should be implemented to respond accordingly to the received details.

* **If the analyzed result is the separated registered user intent ([Custom intent](/Design/Design_Custom_Extension.md#CustomIntent)),**

	Clova sends one of three types of request messages: run extension, run registered intent, or exit extension request types. When the server receives these requests, the server must handle the processes and return the results of starting the extension, handling the designated intent, or exiting the extension.

* **If the analyzed result is the intent provided by Clova as the default ([built-in intent](/Design/Design_Custom_Extension.md#BuiltinIntent)),**

	Clova sends a request message such as a help guide or a positive, negative, or undo action accordingly. When the server receives these requests, the server must return a typical response.

## Step 2. Registering the basic extension information {#Step2}

Access the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a> and register the basic information of the extension.
The main items are as follows:

<ul>
  <li>Extension information
    <ul>
      <li><p><strong>{{ book.DevConsole.cek_id }}</strong>: The unique ID value of the extension. Generally uses a combination of the package name and extension name. Enter the extension ID of Sample Dice as "my.clova.extension.sampledice".</p></li>
    	<li><p><strong>{{ book.DevConsole.cek_invocation_name }}</strong>: The name used to call the extension. Select a word that can be easily recognized by the Clova app or speaker devices. The invocation name used for the Sample Dice extension is "Sample Dice."</p></li>
    </ul>
  </li>
  <li>Setting up a server connection
    <ul>
      <li>
        <p><strong>{{ book.DevConsole.cek_service_endpoint_url }}</strong>: The REST API server of the extension to communicate with Clova. It must be a publicly accessible URI. In Step 1, enter the URI of the server that executed the Sample Dice code.</p>
    		<div class="note">
    			<p><strong>Note!</strong></p>
    			<p>An HTTP connection can be used for testing but an HTTPS connection is required for the official service. The extension server must use ports 80 and 443 for HTTP and HTTPS connections respectively.</p>
    		</div>
      </li>
      <li><p><strong>{{ book.DevConsole.cek_account_linking }}</strong>: Only use when connecting with the membership information of a third-party using an authorization server (based on OAuth 2.0). Set the Sample Dice extension as <strong>{{ book.DevConsole.cek_no }}</strong>.</p></li>
    </ul>
  <li>Deployment information and privacy and compliance information<br />
    <p>This information is required for extension review and deployment. You do not need to enter this information when following the tutorial.</p>
  </li>
</ul>

## Step 3. Registering an interaction model {#Step3}

You can register the interaction model on the <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova developer console</a>.

In this tutorial, the Sample Dice extension rolls one dice by default if the user requests to roll the dice without stating the number of dice. For this tutorial, we will create a simple interaction model to handle the command for throwing a dice. Since the information on the number of dice is not collected, we can register one intent without a slot.

### Creating a new custom intent
Follow the steps below to make a simple intent to roll one dice upon user request.

<ol>
  <li><p>Click <strong>{{ book.DevConsole.cek_edit }}</strong> on the <strong>{{ book.DevConsole.cek_interaction_model }}</strong> item of the Sample Dice extension.</p></li>
  <li><p>Click <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> next to <strong>{{ book.DevConsole.cek_builder_list_title_intent }}</strong>.</p></li>
  <li><p>Enter the name "ThrowDiceIntent" in the input field below<strong>{{ book.DevConsole.cek_builder_new_intent }}</strong>.</p></li>
  <li>
    <p>Press Enter or click <strong>{{ book.DevConsole.cek_builder_new_intent_create }}</strong> next to the input field.</p>
  	<img src="/Develop/Assets/Images/CEK_Tutorial_NewIntent.png" />
  	<div class="note">
  	  <p><strong>Note!</strong></p>
  		<p>The intent name is case-sensitive.</p>
  	</div>
  </li>
</ol>

### Adding a phrase to the sample utterance list
Follow the steps below to configure the user utterances to be processed with the intent created above. We will only add one user utterance for this tutorial, but it is beneficial to have as many sample utterances as possible.

<ol>
  <li><p>Enter "Throw dice" in the <strong>{{ book.DevConsole.cek_builder_intent_expression_title }}</strong> field.</p></li>
  <li><p>Press Enter or click the <img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" /> button.</p></li>
  <li>
    <p>Once you have finished entering all sample utterances, click <strong>{{ book.DevConsole.cek_save }}</strong>.</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_SpeechExample.png" style="margin-top:10px; margin-bottom:10px;" />
  </li>
</ol>

### Building and testing
To check that the interaction model is operating as intended, build and test the interaction model.

<ol>
  <li>
    <p>Click <strong>{{ book.DevConsole.cek_builder_menu_build }}</strong> on the upper-left corner of the <strong>Custom extension</strong> screen.</p>
  	<div class="note">
  	  <p><strong>Note!</strong></p>
  		<p>The build takes around 3-5 minutes to complete. Once the build starts, the button changes to a <strong>{{ book.DevConsole.cek_builder_menu_build_in_progress }}</strong> button. It returns to a <strong>{{ book.DevConsole.cek_builder_menu_build }}</strong> button again once the build is complete.</p>
  	</div>
  </li>
  <li><p>Once the build is complete, click the <strong>{{ book.DevConsole.cek_test }}</strong> menu below the <strong>{{ book.DevConsole.cek_builder_menu_build }}</strong> button.</p></li>
  <li><p>Enter the sentence to test in the <strong>{{ book.DevConsole.cek_builder_test_expression_title }}</strong> field. For example, enter "Roll the dice".</p></li>
  <li><p>Press Enter or click the <strong>{{ book.DevConsole.cek_builder_test_request_test }}</strong> button.</p>
  <li>
    <p>Check whether "ThrowDiceIntent" appears in the <strong>{{ book.DevConsole.cek_builder_test_intent_result }}</strong> field under the <strong>{{ book.DevConsole.cek_builder_test_result_title }}</strong>.</p>
  	<img src="/Develop/Assets/Images/CEK_Tutorial_Test.png" />
  	<div class="note">
    	<p><strong>Note!</strong></p>
    	<p>If you have not registered an extension server URI that can be accessed externally in step 2, <strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong> is displayed as "{{ book.DevConsole.cek_builder_test_no_response }}".</p>
  	</div>
  </li>
</ol>

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
