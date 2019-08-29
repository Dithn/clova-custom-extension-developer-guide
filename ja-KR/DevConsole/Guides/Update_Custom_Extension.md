<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: UpdatingExtension -->

# Custom Extensionをアップデートする

Extensionが審査を通過し、配布が承認されると、そのExtensionは**{{ book.DevConsole.cek_status_prd }}**ステータスになります。Clova Developer Centerはその際、次のようにExtensionの2つのリビジョンを作成します。

* **{{ book.DevConsole.cek_version_service }}**リビジョン：現在、**{{ book.DevConsole.cek_status_prd }}**ステータスのExtensionの元の情報を持つリビジョンです。Extension情報の照会のみできます。
* **{{ book.DevConsole.cek_version_test }}**リビジョン：配布されたExtensionの元の情報をコピーして作成されたリビジョンです。Extensionをアップデートする際に使用されます。

![](/DevConsole/Assets/Images/DevConsole-Extension_List_After_Submission.png)

**{{ book.DevConsole.cek_version_service }}**リビジョンのExtensionは、現在サービス中の内容を反映しているため、修正することができません。Extensionをアップデートするには、コピーされた**{{ book.DevConsole.cek_version_test }}**リビジョンを使用します。Extensionに次の項目に該当するアップデート事項がある場合、**{{ book.DevConsole.cek_version_test }}**リビジョンのExtensionに反映してから、再び審査をリクエストします。

次の項目をアップデートできます。

* [基本情報](/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo)
* [サーバーとの連携情報](/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection)
* [対話モデル](/DevConsole/Guides/Register_Interaction_Model.md)
* [配布情報](/DevConsole/Guides/Deploy_Custom_Extension.md)

審査を通過すると、**{{ book.DevConsole.cek_version_service }}**リビジョンから、アップデートが反映された**{{ book.DevConsole.cek_version_test }}**リビジョンに置き換えられます。それからまた、**{{ book.DevConsole.cek_version_service }}**リビジョンのExtensionをコピーし、**{{ book.DevConsole.cek_version_test }}**リビジョンのExtensionを生成します。

以下の図は、Clova Developer CenterでExtensionがアップデートされる仕組みを示します。

![](/DevConsole/Assets/Images/DevConsole-Branch_Chart_For_Extension_Update.png)

<!-- End of the shared content -->
