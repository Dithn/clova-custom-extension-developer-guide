<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Testing a custom extension
You can test the registered custom extension or interaction model prior to deployment. Follow the steps below to test the custom extension and the interaction model:

* (Custom extension only) [Building an interaction model](#BuildInteractionModel)
* (Custom extension only) [Testing an interaction model](#TestInteractionModel)
* [Testing a custom extension using Clova app](#TestOnClovaApp)

## Building an interaction model {#BuildInteractionModel}

In order to deploy the custom extension, the [interaction model must be registered](/DevConsole/Guides/Register_Interaction_Model.md). The defined interaction must go through the build process to [test](#TestInteractionModel) or use the newly written or updated details. Build the interaction model by completing the following steps:

1. On the list of registered custom extensions, click the **{{ book.DevConsole.cek_edit }}** menu of the custom extension to build the interaction model.<br />
  ![](/DevConsole/Assets/Images/DevConsole-Interaction_Model_Menu.png)
2. On the **{{ book.DevConsole.cek_interaction_model }} : {{ book.DevConsole.cek_builder_header_title_dashboard }}** screen, click **{{ book.DevConsole.cek_builder_menu_build }}** on the top-left corner. Then, the interaction interaction model build will begin.<br />
  It may take around 3-5 minutes depending on the size of the interaction model, etc.<br />
  ![](/DevConsole/Assets/Images/DevConsole-Build_Interaction_Model.png)

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>The build is not canceled even if you move to another menu within the <strong>{{ book.DevConsole.cek_interaction_model }}: {{ book.DevConsole.cek_builder_header_title_dashboard }}</strong> screen. So, you can move between menus or edit details after starting the build.</p>
</div>

## Testing an interaction model {#TestInteractionModel}

Once the [interaction model build](#BuildInteractionModel) is complete, you can test the interaction model. Follow the steps below to test the utterances:

1. Click the **{{ book.DevConsole.cek_test }}** menu below the side menu on the left.<br />
  When you click the menu, the **{{ book.DevConsole.cek_interaction_model }}: {{ book.DevConsole.cek_test }}** screen will be displayed.<br />
  ![](/DevConsole/Assets/Images/DevConsole-Test_Menu.png)
2.**{{ book.DevConsole.cek_builder_test_expression_title }}** On the field, enter the utterance to test and click **{{ book.DevConsole.cek_builder_test_request_test }}**.</li>
  ![](/DevConsole/Assets/Images/DevConsole-Test_Utterance_Example.png)

Once the test is complete, you can check the results. Based on the results below, you must check the following items:

* You must use the **{{ book.DevConsole.cek_builder_test_service_response }}** item to check whether the [registered custom extension](/DevConsole/Guides/Register_Custom_Extension.md) is responding properly.
* You must use the **{{ book.DevConsole.cek_builder_test_intent_result }}** and **{{ book.DevConsole.cek_builder_test_slot_result }}** items to check whether intents and slots are recognized as intended.
* You must use the **{{ book.DevConsole.cek_builder_test_request_json }}** item to check whether there are any problems with the [request messages](/Develop/References/Custom_Extension_Message.md#CustomExtRequestMessage) which the CEK sends to the custom extension. In addition, you can perform the test again after editing the details of the corresponding JSON by pressing **{{ book.DevConsole.cek_builder_test_test_again }}**.
* You must use the **{{ book.DevConsole.cek_builder_test_response_json }}** item to check whether the registered custom extension sends [response messages](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) as intended.

![](/DevConsole/Assets/Images/DevConsole-Test_Result.png)

<!-- Start of the shared content: TestOnClovaApp -->

## Testing a custom extension using Clova app {#TestOnClovaApp}

You can test the custom extensions on the actual client, the Clova app. For this, you must enter the <strong>{{ book.ServiceEnv.OrientedService }} account</strong> of the developer or the tester in the **{{ book.DevConsole.cek_tester }}** field of the page for registering the basic custom extension information. Click **{{ book.DevConsole.cek_save }}** after adding the account to test the custom extension under development on the Clova app where the entered account is verified. To cancel the test from the Clova app, simply delete the entered account information.

![](/DevConsole/Assets/Images/DevConsole-Add_Tester_ID_For_Custom_Extension.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Wait a short while after registering the tester ID to test the custom extension. If you are unable to test the custom extension after about an hour, make an inquiry through the forum or contact the Clova partnership team.</p>
</div>

<!-- End of the shared content -->
