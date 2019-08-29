<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: RemovingExtension -->

# Custom Extensionを中止・削除する

審査をリクエストしていないExtensionは、[基本情報を入力するメニュー](/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo)でそのExtensionを削除できます。

![](/DevConsole/Assets/Images/DevConsole-Remove_Extension.png)

ただし、下記の状態にあるExtensionは削除できません。

* 審査中のExtension
* サービス中のExtension

審査中のExtensionは、審査を取り消ししてから削除できます。

![](/DevConsole/Assets/Images/DevConsole-Cancel_Submission.png)

もし、Extensionが審査を通過し、サービスが開始されている場合には、サービスを一度中止の状態にすることで削除できるようになります。サービスを中止すると、Extensionは**{{ book.DevConsole.cek_status_dev }}**ステータスに戻ります。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>サービスを中止する際には、Clova運営チームの確認が必要です。Extensionを中止するには、<a href="mailto:{{ book.ServiceEnv.ExtensionAdminEmail }}">{{ book.ServiceEnv.ExtensionAdminEmail }}</a>まで連絡してください。</p>
</div>

<!-- End of the shared content -->
