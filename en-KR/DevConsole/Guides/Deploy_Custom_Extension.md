<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Deploying a custom extension
If the [custom extension](/Develop/Guides/Build_Custom_Extension.md) is [registered to the Clova developer console](/DevConsole/Guides/Register_Custom_Extension.md), you can deploy the extension to the Clova service. Once deployed, the custom extension deployed from **{{ book.DevConsole.ManageCustomExtensions }}** is available to be used by general users.

To deploy a custom extension, you must typically complete the following steps:

* [Entering deployment information](#InputDeploymentInfo)
* [Entering privacy and compliance information](#InputComplianceInfo)
* [Requesting a review](#RequestExtensionSubmission)

## Entering deployment information {#InputDeploymentInfo}

You can enter the deployment information after [registering the custom extension](/DevConsole/Guides/Register_Custom_Extension.md) and [registering an interaction model](/DevConsole/Guides/Register_Interaction_Model.md) on the Clova developer console. Select **{{ book.DevConsole.cek_publishing }}** from the menu to register the custom extensions.

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_Deployment_Info_Menu.png)

Enter the deployment information following the example below.

![](/DevConsole/Assets/Images/DevConsole-Input_Custom_Extension_Deployment_Info.png)

This is the information representing your custom extension on **{{ book.DevConsole.ManageCustomExtensions }}**. The details of the entered information are as follows:

* **{{ book.DevConsole.cek_category }}**: The type of the custom extension. Users can use this information to check or search for custom extensions by their type.
* **{{ book.DevConsole.cek_test_instructions }}**: The information used by the review team as a reference for the [custom extension approval](#RequestExtensionSubmission) process where your custom extension is validated. This is not shown to regular users. Follow the instructions to fill in the information.
* Supported countries and regions: Currently, custom extension can be deployed in Korea only.
* **{{ book.DevConsole.cek_full_skill_desc }}**: The description of the custom extension to be provided to users on the **{{ book.DevConsole.ExtensionPage }}**. Follow the instructions to fill in the information.
* **{{ book.DevConsole.cek_short_skill_desc }}**: The short description of the extension to be provided to users on {{ book.DevConsole.StoreHome }} such as promotional information.
* **{{ book.DevConsole.cek_example_phrases }}**: The sample utterance to show users how to use the custom extension. This will be displayed on **{{ book.DevConsole.ExtensionPage }}**. In particular, the first sample utterance will be displayed when showing the custom extension list on {{ book.DevConsole.StoreHome }}.
* **{{ book.DevConsole.cek_keywords }}**: The keywords that can be used by users to find the custom extension of the search result.
* **{{ book.DevConsole.cek_small_icon }}**: The small icon file of the custom extension (108x108 px). This will be displayed in the **{{ book.DevConsole.ManageCustomExtensions }}** menu or the **{{ book.DevConsole.ExtensionPage }}**.
* **{{ book.DevConsole.cek_large_icon }}**: The large icon file of the extension (512x512 px). This will be used as the main icon for the custom extension in the future.

Once the information is filled out, it is displayed on the **{{ book.DevConsole.ManageCustomExtensions }}** menu as shown below:

| {{ book.DevConsole.StoreHome }} | {{ book.DevConsole.ExtensionPage }}   |
|-------------------|-------------------|
| ![Custom extension List](/DevConsole/Assets/Images/DevConsole-Store_UI_Example-Custom_Extension_Store_Home.png) | ![Custom extension Details](/DevConsole/Assets/Images/DevConsole-Store_UI_Example-Custom_Extension_Page.png) |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>Some of the information displayed on the <strong>{{ book.DevConsole.ExtensionPage }}</strong> is based on the information entered when <a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">registering basic information for the extension</a>.</p>
</div>

## Entering privacy and compliance information {#InputComplianceInfo}

As the last step of entering the information required to deploy the custom extension, you must enter the details on privacy and compliance. Select **{{ book.DevConsole.cek_privacy }}** from the menu to register the custom extensions.

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_Policy_Menu.png)

Enter the information following the example below.

![](/DevConsole/Assets/Images/DevConsole-Input_Custom_Extension_Policy.png)

* **{{ book.DevConsole.cek_allow_purchase }}**: Select **{{ book.DevConsole.cek_yes }}** if the user must make a payment when using the custom extension.
* **{{ book.DevConsole.cek_use_personal_info }}**: Select **{{ book.DevConsole.cek_yes }}** if the custom extension collects personal information from users.
* **{{ book.DevConsole.cek_child_directed }}**: Select **{{ book.DevConsole.cek_yes }}** if the custom extension can be used by minors.
* **{{ book.DevConsole.cek_privacy_policy_url }}**: Input information on privacy policy, if the custom extension collects personal information. This will be displayed at the bottom of the custom extension description page.
* **{{ book.DevConsole.cek_terms_of_use }}**: Enter the URI of the disclaimer for the custom extension. This will be displayed at the bottom of the custom extension description page with the URI of the privacy policy.

The URIs entered in **{{ book.DevConsole.cek_privacy_policy_url }}** and **{{ book.DevConsole.cek_terms_of_use }}** will be displayed on the **{{ book.DevConsole.ExtensionPage }}** as follows:

![](/DevConsole/Assets/Images/DevConsole-Store_UI_Example-Extension_Policy.png)

<!-- Start of the shared content: RequestExtensionSubmission -->

## Requesting a review {#RequestExtensionSubmission}

Once you have filled out the [deployment information](#InputDeploymentInfo) and the [privacy and compliance information](#InputComplianceInfo) of Custom extension, you can request a review of your registered extension as the last step. Then a Clova administrator reviews the extension such as its registered information, actual execution, and suitability.

* If the extension is functioning properly and no issues are found during the review, it will pass the review and you will be able to deploy the extension immediately.
* If an execution error is found during the review or a critical issue is found within potential user scenarios, the request for deployment will be rejected by the administrator. Then the extension will go back to the previous status.

![](/DevConsole/Assets/Images/DevConsole-Extension_Submission_Process.png)

You can request for a review by clicking the **{{ book.DevConsole.cek_request_submit }}** menu on the registered extensions list.

![](/DevConsole/Assets/Images/DevConsole-Submit_Extension_1.png)

Or, click **{{ book.DevConsole.cek_request_submit }}** at the bottom of the screen for entering [privacy and compliance information](#InputComplianceInfo).

![](/DevConsole/Assets/Images/DevConsole-Submit_Extension_2.png)

If you click **{{ book.DevConsole.cek_request_submit }}**, you can leave information on the review request for the administrator. If it is your first review request for the extension, you can mention that and describe the extension. If it is your second review request for the extension by correcting the extension, enter the improved details or enter whether the issues have been corrected.

![](/DevConsole/Assets/Images/DevConsole-Submission_Request_Message.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>You cannot edit the extension information and interaction model during the review stage.</p>
</div>

The Skill review will be carried out in a separate environment before publishing it to **{{ book.DevConsole.ManageCustomExtensions }}**. If the service requires [account linking](/Develop/Guides/Link_User_Account.md), then you must input the account information for testing under the **{{ book.DevConsole.cek_test_instructions }}** when you [input the deployment information](#InputDeploymentInfo).

The following items will be evaluated during the review:

* [User scenario](/Design/Design_Custom_Extension.md#MakeUseCaseScenarioScript) and content verification
  * Check whether there are unnatural sentences in the dialogue.
  * Check whether there are sensitive or prohibited words in the utterance data used in scenarios.
  * Check whether the [Guidelines for providing content](/Design/Rules_For_Content.md) are observed.
  * Check specific parts of the custom extension if the extension [links to a user account](/Develop/Guides/Link_User_Account.md).
* Verify actions
  * Check whether the custom extension service uses appropriate terminology.
  * Verify the interaction model such as intents and slots.
  * Check whether the service is provided in accordance with the [detailed goals](/Design/Design_Custom_Extension.md#SettingGoal) of the skill.
* Deployment information verification
  * Check whether deployment information, such as the description, category, or search keyword of the custom extension, is correctly entered for the custom extension.
  * Check whether the skill is operating properly according to the entered policies such as the privacy policy.

You can click the **{{ book.DevConsole.cek_cancel_review }}** menu anytime during the review process to cancel the review request. If you cancel the review request, the extension will go back to the previous status.

![](/DevConsole/Assets/Images/DevConsole-Cancel_Submission.png)

If the extension does not pass the review, the **{{ book.DevConsole.cek_status }}** of the extension changes to **{{ book.DevConsole.cek_status_rejected }}**. This status is identical to the **{{ book.DevConsole.cek_status_dev }}** status and you can make a review request again.

![](/DevConsole/Assets/Images/DevConsole-Extension_Submission_Rejected.png)

At this time, you can click the **{{ book.DevConsole.cek_view }}** menu on the **{{ book.DevConsole.cek_message }}** to check feedback on the review.

![](/DevConsole/Assets/Images/DevConsole-Show_Submission_Feedback.png)

<!-- End of the shared content -->
