<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: UpdatingExtension -->

# Updating a custom extension

If the extension passes the review and deployment is approved, the extension is changed to a **{{ book.DevConsole.cek_status_prd }}** status. At this time, the Clova developer console makes two versions of the extension as shown below.

* **{{ book.DevConsole.cek_version_service }}** version: Original version containing the original information of the extension in the **{{ book.DevConsole.cek_status_prd }}** status. This version of the extension is view-only.
* **{{ book.DevConsole.cek_version_test }}** version: Copied version of the original which is used to update the extension.

![](/DevConsole/Assets/Images/DevConsole-Extension_List_After_Submission.png)

The extension information in the **{{ book.DevConsole.cek_version_service }}** version includes the details that are currently in service, and cannot be edited. Therefore, the extension must be updated using the copied **{{ book.DevConsole.cek_version_test }}** version. When there is an update on the items shown below, you can implement it in the **{{ book.DevConsole.cek_version_test }}** version and request for another review.

Updates may include:

* [Basic information](/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo)
* [Server connection information](/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection)
* [Interaction model](/DevConsole/Guides/Register_Interaction_Model.md)
* [Deployment information](/DevConsole/Guides/Deploy_Custom_Extension.md)

Once the review is passed, the**{{ book.DevConsole.cek_version_service }}** version is replaced with the updated **{{ book.DevConsole.cek_version_test }}** version. Then, the extension information from the **{{ book.DevConsole.cek_version_service }}** version is copied again to create the new **{{ book.DevConsole.cek_version_test }}** version of the extension information.

The image below shows an overview of an extension update in the Clova developer console.

![](/DevConsole/Assets/Images/DevConsole-Branch_Chart_For_Extension_Update.png)

<!-- End of the shared content -->
