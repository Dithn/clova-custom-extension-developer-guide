<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Custom Extensionを配布する
[Custom Extension](/Develop/Guides/Build_Custom_Extension.md)を[Clova Developer Centerに登録](/DevConsole/Guides/Register_Custom_Extension.md)すると、Custom ExtensionをClovaサービスに配布できます。配布されたCustom Extensionは、エンドユーザーが**{{ book.DevConsole.ManageCustomExtensions }}**から使用することができます。

Custom Extensionの配布は、通常次の順で行います。

* [配布情報を入力する](#InputDeploymentInfo)
* [プライバシーポリシーおよびコンプライアンス情報を入力する](#InputComplianceInfo)
* [審査をリクエストする](#RequestExtensionSubmission)

## 配布情報を入力する {#InputDeploymentInfo}

Clova Developer Centerで[Custom Extensionを登録](/DevConsole/Guides/Register_Custom_Extension.md)し、[対話モデルを登録](/DevConsole/Guides/Register_Interaction_Model.md)すると、配布情報を入力できます。Custom Extensionの登録メニューで**{{ book.DevConsole.cek_publishing }}**を選択します。

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_Deployment_Info_Menu.png)

次のように配布情報を入力します。

![](/DevConsole/Assets/Images/DevConsole-Input_Custom_Extension_Deployment_Info.png)

ユーザーにCustom Extensionを説明するための情報です。**{{ book.DevConsole.ManageCustomExtensions }}**からユーザーに提供されます。次の情報を入力する必要があります。

* **{{ book.DevConsole.cek_category }}**：Custom Extensionのカテゴリです。ユーザーがカテゴリごとにCustom Extensionのリストを確認したり、検索する際に利用されます。
* **{{ book.DevConsole.cek_test_instructions }}**：[Custom Extensionを審査](#RequestExtensionSubmission)段階でClova事務局がCustom Extensionを確認するときに必要な参考情報です。エンドユーザーには表示されません。案内に従って作成します。
* サービスを提供する国および地域：現在、日本でのみCustom Extensionを配布できます。
* **{{ book.DevConsole.cek_full_skill_desc }}**：**{{ book.DevConsole.ExtensionPage }}**でユーザーに提供するCustom Extensionの説明です。案内に従って作成します。
* **{{ book.DevConsole.cek_short_skill_desc }}**：{{ book.DevConsole.StoreHome }}でプロモーションなどの案内を表示する際に使用される説明です。
* **{{ book.DevConsole.cek_example_phrases }}**：ユーザーがCustom Extensionをどのように使用できるかを示す例です。**{{ book.DevConsole.ExtensionPage }}**に表示されます。特に、一番目の例は、**{{ book.DevConsole.StoreHome }}**でCustom Extensionのリストを表示する際に使用されます。
* **{{ book.DevConsole.cek_keywords }}**：ユーザーが特定のキーワードでCustom Extensionを検索すると、その検索結果にCustom Extensionが含まれます。
* **{{ book.DevConsole.cek_small_icon }}**：小サイズ（108x108ピクセル）のCustom Extensionのアイコンファイルです。**{{ book.DevConsole.ManageCustomExtensions }}**と**{{ book.DevConsole.ExtensionPage }}**に表示されます。
* **{{ book.DevConsole.cek_large_icon }}**：大サイズ（512x512ピクセル）のCustom Extensionのアイコンファイルです。今後使用される予定です。

入力された情報は、**{{ book.DevConsole.ManageCustomExtensions }}**に次のように表示されます。

| {{ book.DevConsole.StoreHome }} | {{ book.DevConsole.ExtensionPage }}   |
|-------------------|-------------------|
| ![Custom Extension List](/DevConsole/Assets/Images/DevConsole-Store_UI_Example-Custom_Extension_Store_Home.png) | ![Custom Extension Details](/DevConsole/Assets/Images/DevConsole-Store_UI_Example-Custom_Extension_Page.png) |

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p><strong>{{ book.DevConsole.ExtensionPage }}</strong>に表示される一部の情報には、登録されている<a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">Extensionの基本情報</a>が使用されます。</p>
</div>

## プライバシーポリシーおよびコンプライアンス情報を入力する {#InputComplianceInfo}

Custom Extensionの配布に必要な情報を入力する最終ステップです。プライバシーポリシーおよびコンプライアンス関連の内容を入力します。Custom Extensionの登録メニューで**{{ book.DevConsole.cek_privacy }}**を選択します。

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_Policy_Menu.png)

以下のように情報を入力します。

![](/DevConsole/Assets/Images/DevConsole-Input_Custom_Extension_Policy.png)

* **{{ book.DevConsole.cek_allow_purchase }}**：Custom Extensionを使用する際、ユーザーが決済をしたり支払いをする場面がある場合、**{{ book.DevConsole.cek_yes }}**を選択します。
* **{{ book.DevConsole.cek_use_personal_info }}**：Custom Extensionがユーザーの個人情報を取得する場合、**{{ book.DevConsole.cek_yes }}**を選択します。
* **{{ book.DevConsole.cek_child_directed }}**：未成年者のCustom Extension使用を許可する場合、**{{ book.DevConsole.cek_yes }}**を選択します。
* **{{ book.DevConsole.cek_privacy_policy_url }}**：Custom Extensionが個人情報を取得する場合、プライバシーポリシーを提供するページを入力します。Custom Extension説明ページの一番下に表示されます。
* **{{ book.DevConsole.cek_terms_of_use }}**：Custom Extensionに関する免責条項を提供するページを入力します。プライバシーポリシーのURIと同様に、Custom Extension説明ページの一番下に表示されます。

**{{ book.DevConsole.cek_privacy_policy_url }}**と**{{ book.DevConsole.cek_terms_of_use }}**に入力された内容は、**{{ book.DevConsole.ExtensionPage }}**で次のように表示されます。

![](/DevConsole/Assets/Images/DevConsole-Store_UI_Example-Extension_Policy.png)

<!-- Start of the shared content: RequestExtensionSubmission -->

## 審査をリクエストする {#RequestExtensionSubmission}

Custom Extensionの[配布情報](#InputDeploymentInfo)と[プライバシーポリシーおよびコンプライアンス情報](#InputComplianceInfo)まで入力すると、登録したExtensionの審査をリクエストできるようになります。Clova事務局は、登録されたExtensionの情報、実際の動作確認と適合性などを審査します。

* Extensionが正常に動作し、検討の結果特に異常がない場合、Extensionは審査を通過します。審査を通過すると、直ちにExtensionを配布できるようになります。
* もし審査中に実行エラーが発生したり、ユーザーシナリオの深刻な問題が見つかると、Clova事務局により配布のリクエストがリジェクトされ、審査をリクエストする前に戻ります。

![](/DevConsole/Assets/Images/DevConsole-Extension_Submission_Process.png)

Extensionの審査をリクエストするには、登録したExtensionのリストで**{{ book.DevConsole.cek_request_submit }}**メニューをクリックします。

![](/DevConsole/Assets/Images/DevConsole-Submit_Extension_1.png)

または[プライバシーポリシーおよびコンプライアンス情報](#InputComplianceInfo)を入力する画面の一番下にある**{{ book.DevConsole.cek_request_submit }}**ボタンをクリックして、リクエストすることもできます。

![](/DevConsole/Assets/Images/DevConsole-Submit_Extension_2.png)

**{{ book.DevConsole.cek_request_submit }}**をクリックすると、以下のように審査申請に関する情報をClova事務局に送信できます。Extensionの最初の審査リクエストの場合、最初の審査リクエストというメッセージと、Extensionを説明するメッセージを記載します。Extensionを修正して再審査をリクエストする場合、改善事項やリジェクト意見の反映有無を入力します。

![](/DevConsole/Assets/Images/DevConsole-Submission_Request_Message.png)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>審査中には、Extensionの情報と対話モデルを修正できません。</p>
</div>

スキルの審査は、**{{ book.DevConsole.ManageCustomExtensions }}**に反映する前に、Clova事務局にて実施します。もし、[ユーザーアカウントの連携](/Develop/Guides/Link_User_Account.md)が必要なサービスの場合は、[配布情報を入力](#InputDeploymentInfo)する際に、テスト用のアカウント名およびパスワードを**{{ book.DevConsole.cek_test_instructions }}**欄に入力していただく必要があります。

審査の際に確認する評価項目は次のとおりです。

* [ユーザーシナリオ](/Design/Design_Custom_Extension.md#MakeUseCaseScenarioScript)およびコンテンツの検証
  * 会話に不自然なところがないか確認します。
  * シナリオで使用される発話データに、禁止用語や差別用語などが含まれていないか確認します。
  * [コンテンツの提供に関するガイドライン](/Design/Rules_For_Content.md)を守っているか確認します。
  * Extensionが[ユーザーアカウントと連携](/Develop/Guides/Link_User_Account.md)する場合、サービスに特化した部分をさらに検討することがあります。
* 動作の検証
  * Custom Extensionがサービスに適切な用語を使用しているか確認します。
  * インテント、スロットなどの対話モデルを検証します。
  * スキルの[具体的な目標](/Design/Design_Custom_Extension.md#SettingGoal)に合ったサービスを提供しているか確認します。
* 配布情報の検証
  * Custom Extensionの説明、カテゴリ、検索キーワードなどの配布情報が正しく入力されているか確認します。
  * スキルが、配布情報に入力された利用規約やプライバシーポリシーに違反していないことを確認します。

審査中に**{{ book.DevConsole.cek_cancel_review }}**メニューをクリックすると、いつでも審査リクエストを取り消しできます。審査のリクエストをキャンセルすると、前のステータスに戻ります。

![](/DevConsole/Assets/Images/DevConsole-Cancel_Submission.png)

審査を通過しなかった場合、Extensionの**{{ book.DevConsole.cek_status }}**が **{{ book.DevConsole.cek_status_rejected }}**に変更されます。これは**{{ book.DevConsole.cek_status_dev }}**と同じステータスで、再び審査をリクエストできます。

![](/DevConsole/Assets/Images/DevConsole-Extension_Submission_Rejected.png)

その際、**{{ book.DevConsole.cek_message }}**の**{{ book.DevConsole.cek_view }}**メニューをクリックすると、審査のフィードバックを確認できます。

![](/DevConsole/Assets/Images/DevConsole-Show_Submission_Feedback.png)

<!-- End of the shared content -->
