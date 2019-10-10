<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Custom Extensionを登録する
[Custom Extension](/Develop/Guides/Build_Custom_Extension.md)を開発中もしくは開発済みの場合、それを[Clova Developer Center](/DevConsole/ClovaDevConsole_Overview.md)に登録する必要があります。CEKのメニューページの下にある**{{ book.DevConsole.cek_new_skill }}**ボタンをクリックすると、新規のCustom Extensionを登録できます。

![](/DevConsole/Assets/Images/DevConsole-First_Look_of_Extension_List.png)

Custom Extensionの登録は、通常次の順で行います。

1. [利用規約および個人情報の取得に同意する](#AgreeTermsOfUse)
2. [Custom Extensionの基本情報を入力する](#InputExtensionInfo)
3. [サーバー設定を行う](#SetServerConnection)
  * [アカウント連携を設定する](#SetAccountLinking)

<!-- Start of the shared content: AgreeTermsOfUse -->

## 利用規約および個人情報の取得に同意する {#AgreeTermsOfUse}

Custom Extensionを登録するには、先にCEKのAPIサービスの利用規約と個人情報の取得に同意する必要があります。利用規約および個人情報の取得に関する内容は、**最初の一度のみ表示**され、同意した後は表示されません。

![](/DevConsole/Assets/Images/DevConsole-Agree_Terms_of_Use_and_Collecting_Personal_Info.png)

<!-- End of the shared content -->

## Custom Extensionの基本情報を入力する {#InputExtensionInfo}

Custom Extensionを登録する最初のステップは、登録するCustom Extensionの基本情報を入力することです。Custom Extensionの基本情報は、Clova Developer CenterでCustom Extensionを作成するための必須情報です。Custom Extensionの基本情報を入力すると、CEKメニューで作成したCustom Extensionに自由アクセスや編集が可能になります。

次の順でCustom Extensionを登録します。

![](/DevConsole/Assets/Images/DevConsole-Create_New_Custom_Extension.png)

1. **{{ book.DevConsole.cek_type }}**項目で、登録するCustom Extensionのタイプを選択します。<br />
  Custom Extensionのタイプを選択すると、該当する入力フィールドが表示されます。
2. **{{ book.DevConsole.cek_lang }}**項目で、Custom Extensionで使用する言語を選択します。現在、**{{ book.DevConsole.supported_languages }}**のみサポートされています。
3. **Extension ID**、**スキル名**、**呼び出し名**、**開発会社**を次の項目に入力します。
  1. **{{ book.DevConsole.cek_id }}**<br />
    Custom Extension固有IDを入力します。です。リバースドメインネームの形式（例：com.yourdomain.extension.pizzabot)を入力してください。
  2. **{{ book.DevConsole.cek_name }}**<br />
    Custom Extensionの名前を入力します。後で**{{ book.DevConsole.ManageCustomExtensions }}**に表示されます。
  3. **{{ book.DevConsole.cek_invocation_name }}**<br />
    ：ユーザーがCustom Extensionを呼び出す際に呼ぶ名前を入力します。1つから最大3つまで{{ book.DevConsole.cek_invocation_name }}を登録できます。お持ちのサービス、会社および組織の名前を使用できますが、ユーザーにとって呼びやすい、シンプルな言葉を指定することをお勧めします。汎用的な言葉、他社の名前やサービスに該当する言葉は使用できません。**{{ book.DevConsole.cek_invocation_name }}**は、Custom Extensionを審査する際にチェックされます。
  4. **{{ book.DevConsole.cek_provider }}**<br />
    Custom Extensionを作成した主体（会社や個人）の名前またはニックネームを入力します。後で**{{ book.DevConsole.ManageCustomExtensions }}**に表示され、Custom Extensionを審査する際にチェックされます。
4. Extensionが[AudioPlayer]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }})ディレクティブを使用する場合、**{{ book.DevConsole.cek_audioplayer }}**項目で**{{ book.DevConsole.cek_yes }}**を選択します。Custom Extensionがオーディオストリーミングサービスを提供する際に使用されます。
5. **{{ book.DevConsole.cek_email }}**項目に、連絡可能なメールアドレスを入力します。
6. **{{ book.DevConsole.cek_tester }}**項目に、Custom Extensionのテストに使用する{{ book.ServiceEnv.OrientedService }}アカウントを入力します。<br />
  必須ではなく、後で[Extensionをテスト](/DevConsole/Guides/Test_Custom_Extension.md)する際に入力することもできます。
7. Custom Extensionの基本情報をすべて入力したら、**{{ book.DevConsole.cek_create }}**ボタンをクリックします。

Custom Extensionの基本情報をすべて入力すると、作成されたCustom Extensionの情報を編集する画面に切り替わります。ページの下にある**{{ book.DevConsole.cek_save }}**ボタンをクリックして、入力中の内容を自由に保存できます。また、CEKのメニューで登録されたCustom Extensionのリストを確認することもできます。

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_List_After_Creation.png)

## サーバー設定を行う {#SetServerConnection}

Custom ExtensionはCEKとHTTPSで通信します。その際、CEKはCustom ExtensionにHTTPリクエストを送り、Custom ExtensionはCEKにHTTPレスポンスを返します。CEKがCustom ExtensionにHTTPリクエストを送るには、Clova Developer Centerでサーバーとの連携を設定する必要があります。[Custom Extensionの基本情報を入力](#InputExtensionInfo)すると、作成されたCustom Extensionに対しサーバーとの連携を設定できます。

Custom Extensionのサーバーを登録するには、先にCustom Extensionのサーバーと通信できるか確認する必要があります。次の例のように、簡単なcurlコマンドで通信状況を確認できます。

{% raw %}
```bash
$ curl "https://example.com/pizzabot" -X POST
```
{% endraw %}

次の順でサーバー設定を行います。

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_Server_Settings.png)

1. Extensionの情報入力UIで、上にある**{{ book.DevConsole.cek_configuration }}**タブをクリックします。
2. Custom ExtensionサーバーのURI（エンドポイント）を**{{ book.DevConsole.cek_service_endpoint_url }}**項目に入力します。
  <div class="note">
    <p>**メモ**</p>
    <p>テスト段階ではHTTPも使用できますが、正式なサービスのためにはHTTPSを使用する必要があります。Custom Extensionのサーバーは、HTTPで80ポート、HTTPSで443ポートに設定してください。</p>
  </div>
3. [アカウント連携](#SetAccountLinking)を必要とする場合、**{{ book.DevConsole.cek_account_linking }}**項目で**{{ book.DevConsole.cek_yes }}**を選択します。
4. **{{ book.DevConsole.cek_ssl_certificate }}**項目のオプションボタンをクリックします。Custom Extensionを提供するサーバーは、必ず信頼された認証局から発行された証明書を使用してください。（自己署名証明書は使用できません）
5. サーバーとの連携に関する内容を入力して、**{{ book.DevConsole.cek_save }}**ボタンをクリックします。</li>

### アカウント連携を設定する {#SetAccountLinking}

Custom Extensionで提供するサービスのアカウントがClovaのユーザーアカウントとの連携を必要とする場合、[サーバー設定を行う](#SetServerConnection)の内、[アカウント連携](/Develop/Guides/Link_User_Account.md)に関連情報を入力する必要があります。

次の順で、アカウント連携の設定に[必要な情報](/Develop/Guides/Link_User_Account.md#RegisterAccountLinkingInfo)を入力します。

<img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_1.png" />

1. **{{ book.DevConsole.cek_account_linking }}**項目で**{{ book.DevConsole.cek_yes }}**を選択します。
2. ユーザーにアカウント認証UIを提供する認証URIを**{{ book.DevConsole.cek_authorization_url }}**項目に入力します。<br />
  ユーザーがCustom Extensionをアクティブにすると、このページに移動します。
3. ユーザーが自身のアカウントをすぐに設定できるようにする場合には、**{{ book.DevConsole.cek_configuration_url }}**項目にアカウント設定ページのURIを入力します。
4. ユーザーアカウント認証を行うとき、HTTPリクエストに必要な**{{ book.DevConsole.cek_client_id }}**を入力します。<br />
  クライアントIDは、[認可サーバーを構築](/Develop/Guides/Link_User_Account.md#BuildAuthServer)する際に生成した値です。
5. **{{ book.DevConsole.cek_privacy_policy_url }}**項目に、Custom Extensionが提供するサービスのプライバシーポリシーのURIを入力します。<br />
  このページの内容は、後で**{{ book.DevConsole.ManageCustomExtensions }}**に表示されます。
6. **{{ book.DevConsole.cek_authorization_url }}**または**{{ book.DevConsole.cek_privacy_policy_url }}**に登録したページが別のドメインから必要なリソースを読み込む場合、**{{ book.DevConsole.cek_domain_list }}**項目に必要なドメインを追加します。
7. （任意）アカウント連携の際に発行されるアクセストークンのスコープをあらかじめ定義している場合、**{{ book.DevConsole.cek_scope }}**項目に追加します。<br />
  <img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_2.png" />
8. **{{ book.DevConsole.cek_access_token_uri }}**項目に、サービスのアクセストークンを発行できるURIを入力します。<br />
  現在、**認可グラントタイプはAuthorization code grantのみサポート**されています。
9. **{{ book.DevConsole.cek_refresh_token_uri }}**項目に、サービスのアクセストークンを更新できるURIを入力します。
10. サービスのアクセストークンを取得するときに、HTTPリクエストに必要な**{{ book.DevConsole.cek_client_secret }}**を入力します。<br />
  クライアントシークレットは、[認可サーバーを構築](/Develop/Guides/Link_User_Account.md#BuildAuthServer)する際に生成した値です。
11. **{{ book.DevConsole.cek_client_authentication_scheme }}**は、次のうち認可サーバーのインターフェースの実装に適した値を設定します。
  *. **HTTP Basic（推奨）**：サービスのアクセストークンを取得するために資格情報をヘッダーに入力される場合
  * **Credentials in request body**：サービスのアクセストークンを取得するために資格情報をボディに入力される場合

<div id="RedirectURI" class="note">
  <p><strong>メモ</strong></p>
  <p>アカウント認証を済ませた後、クライアントがリダイレクトするURIは<code>{{ book.ServiceEnv.RedirectURIforAccountLinking }}</code>で、<strong>{{ book.DevConsole.cek_redirect_urls }}</strong>項目で確認できます。</strong><a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">認可サーバー</a>は、この値がクライアントから渡される<code>redirect_uri</code>の値と一致するか検証する必要があります。</p>
  <img src="/DevConsole/Assets/Images/DevConsole-Redirect_URI_for_Extension_Accoun_Linking.png" />
</div>
