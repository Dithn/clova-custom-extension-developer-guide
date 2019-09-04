<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: RemovingExtension -->

# Disabling and deleting a custom extension

You can delete an extension prior to the review request from the corresponding menus for entering the [basic extension information](/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo).

![](/DevConsole/Assets/Images/DevConsole-Remove_Extension.png)

However, you cannot delete extensions in the following statuses:

* Extension under review
* Extension in service

You can always cancel a review and delete the extensions under review.

![](/DevConsole/Assets/Images/DevConsole-Cancel_Submission.png)

If the extension is in service after passing the review, you can delete it after canceling the service. If you cancel the service, the status of the extension is changed to the **{{ book.DevConsole.cek_status_dev }}** status.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>A check from the Clova operations team is required to cancel a service. To stop the service of an extension, contact the operations team at <a href="mailto:{{ book.ServiceEnv.ExtensionAdminEmail }}">{{ book.ServiceEnv.ExtensionAdminEmail }}</a>.</p>
</div>

<!-- End of the shared content -->
