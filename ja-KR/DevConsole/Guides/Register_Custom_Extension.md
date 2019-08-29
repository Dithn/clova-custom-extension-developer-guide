<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Custom Extensionを登録する
[Custom Extension](/Develop/Guides/Build_Custom_Extension.md)を開発中もしくは開発済みの場合、それを[Clova Developer Center](/DevConsole/ClovaDevConsole_Overview.md)に登録する必要があります。CEKのメニューページの下にある**{{ book.DevConsole.cek_new_skill }}**ボタンをクリックすると、新規のCustom Extensionを登録できます。

![](/DevConsole/Assets/Images/DevConsole-First_Look_of_Extension_List.png)

Custom Extensionの登録は、通常次の順で行います。

<ol>
  <li><a href="#AgreeTermsOfUse">利用規約および個人情報の取得に同意する</a></li>
  <li><a href="#InputExtensionInfo">Custom Extensionの基本情報を入力する</a></li>
  <li><a href="#SetServerConnection">サーバー設定を行う</a>
    <ul>
      <li><a href="#SetAccountLinking">アカウント連携を設定する</a></li>
    </ul>
  </li>
</ol>

<!-- Start of the shared content: AgreeTermsOfUse -->

## 利用規約および個人情報の取得に同意する {#AgreeTermsOfUse}

Custom Extensionを登録するには、先にCEKのAPIサービスの利用規約と個人情報の取得に同意する必要があります。利用規約および個人情報の取得に関する内容は、**最初の一度のみ表示**され、同意した後は表示されません。

![](/DevConsole/Assets/Images/DevConsole-Agree_Terms_of_Use_and_Collecting_Personal_Info.png)

<!-- End of the shared content -->

## Custom Extensionの基本情報を入力する {#InputExtensionInfo}

Custom Extensionを登録する最初のステップは、登録するCustom Extensionの基本情報を入力することです。Custom Extensionの基本情報は、Clova Developer CenterでCustom Extensionを作成するための必須情報です。Custom Extensionの基本情報を入力すると、CEKメニューで作成したCustom Extensionに自由アクセスや編集が可能になります。

次の順でCustom Extensionを登録します。

![](/DevConsole/Assets/Images/DevConsole-Create_New_Custom_Extension.png)

<ol>
  <li><strong>{{ book.DevConsole.cek_type }}</strong>項目で、登録するCustom Extensionのタイプを選択します。Custom Extensionのタイプを選択すると、該当する入力フィールドが表示されます。</li>
  <li><strong>{{ book.DevConsole.cek_lang }}</strong>項目で、Custom Extensionで使用する言語を選択します。現在、<strong>{{ book.DevConsole.ko_KR }}</strong>のみサポートされています。</li>
  <li><strong>Extension ID</strong>、<strong>スキル名</strong>、<strong>呼び出し名</strong>を次の項目に入力します。
    <ol>
      <li><strong>{{ book.DevConsole.cek_id }}</strong>：Custom Extension固有IDです。リバースドメインネームの形式（例：com.yourdomain.extension.pizzabot）を入力します。</li>
      <li><strong>{{ book.DevConsole.cek_name }}</strong>：Custom Extension名です。<strong>{{ book.DevConsole.ManageCustomExtensions }}</strong>に表示されます。</li>
      <li><strong>{{ book.DevConsole.cek_invocation_name }}</strong>：ユーザーがCustom Extensionを呼び出す際に呼ぶ名前です。1つから最大3つまで{{ book.DevConsole.cek_invocation_name }}を登録できます。お持ちのサービス、会社および組織の名前を使用できますが、ユーザーにとって呼びやすい、シンプルな言葉を指定することをお勧めします。汎用的な言葉、他社の名前やサービスに該当する言葉は使用できません。<strong>{{ book.DevConsole.cek_invocation_name }}</strong>は、Custom Extensionを審査する際にチェックされます。</li>
      <li><strong>{{ book.DevConsole.cek_provider }}</strong>：Custom Extensionを作成した主体（会社や個人）の名前またはニックネームを入力します。<strong>{{ book.DevConsole.ManageCustomExtensions }}</strong>に表示され、Custom Extensionを審査する際にチェックされます。</li>
    </ol>
  </li>
  <li>Extensionが<a href="{{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a>ディレクティブを使用する場合、<strong>{{ book.DevConsole.cek_audioplayer }}</strong>項目で<strong>{{ book.DevConsole.cek_yes }}</strong>を選択します。Custom Extensionがオーディオストリーミングサービスを提供する際に使用されます。</li>
  <li><strong>{{ book.DevConsole.cek_email }}</strong>項目に、連絡可能なメールアドレスを入力します。</li>
  <li><strong>{{ book.DevConsole.cek_tester }}</strong>項目にCustom Extensionのテストに使用する{{ book.ServiceEnv.OrientedService }}アカウントを入力します。必須ではなく、後ほど<a href="/DevConsole/Guides/Test_Custom_Extension.md">のExtensionをテスト</a>する際に入力することもできます。</li>
  <li>Custom Extensionの基本情報をすべて入力したら、<strong>{{ book.DevConsole.cek_create }}</strong>ボタンをクリックします。</li>
</ol>

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

<ol>
  <li>Extensionの情報入力UIで、上にある<strong>{{ book.DevConsole.cek_configuration }}</strong>タブをクリックします。</li>
  <li>Custom ExtensionサーバーのURI（エンドポイント）を<strong>{{ book.DevConsole.cek_service_endpoint_url }}</strong>項目に入力します。
    <div class="note">
      <p><strong>メモ</strong></p>
      <p>テスト段階ではHTTPも使用できますが、正式なサービスのためにはHTTPSを使用する必要があります。Custom Extensionのサーバーは、HTTPで80ポート、HTTPSで443ポートに設定してください。</p>
    </div>
  </li>
  <li>Custom Extensionが提供するサービスのアカウントが、Clovaのユーザーアカウントとの連携を必要とする場合、<strong>{{ book.DevConsole.cek_account_linking }}</strong>項目で<strong>{{ book.DevConsole.cek_yes }}</strong>を選択します。アカウント連携の詳細については、<a href="#SetAccountLinking">アカウント連携を設定する</a>を参照してください。</li>
  <li><strong>{{ book.DevConsole.cek_ssl_certificate }}</strong>項目のオプションボタンをクリックします。Custom Extensionを提供するサーバーは、必ず信頼された認証局から発行された証明書を使用してください。（自己署名証明書は使用できません）</li>
  <li>サーバーとの連携に関する内容を入力して、<strong>{{ book.DevConsole.cek_save }}</strong>ボタンをクリックします。</li>
</ol>

### アカウント連携を設定する {#SetAccountLinking}

Custom Extensionで提供するサービスのアカウントがClovaのユーザーアカウントとの連携を必要とする場合、[サーバー設定を行う](#SetServerConnection)の内、[アカウント連携](/Develop/Guides/Link_User_Account.md)に関連情報を入力する必要があります。

次の順で、アカウント連携の設定に[必要な情報](/Develop/Guides/Link_User_Account.md#RegisterAccountLinkingInfo)を入力します。

<ol>
  <img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_1.png" />
  <li><strong>{{ book.DevConsole.cek_account_linking }}</strong>項目で<strong>{{ book.DevConsole.cek_yes }}</strong>を選択します。</li>
  <li>ユーザーにアカウント認証のためのUIを提供する認証URIを、<strong>{{ book.DevConsole.cek_authorization_url }}</strong>項目に入力します。ユーザーがCustom Extensionをアクティブにすると、このページに移動します。</li>
  <li>ユーザーが自身のアカウントを即座に設定できるようにする場合には、<strong>{{ book.DevConsole.cek_configuration_url }}</strong>項目にアカウント設定ページのURIを入力します。</li>
  <li>ユーザーアカウント認証を行うとき、HTTPリクエストに必要な<strong>{{ book.DevConsole.cek_client_id }}</strong>を入力します。クライアントIDは、<a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">認可サーバーを構築</a>する際に生成した値です。</li>
  <li><strong>{{ book.DevConsole.cek_privacy_policy_url }}</strong>項目に、Custom Extensionが提供するサービスのプライバシーポリシーが提供されるURIを入力します。このページの内容は、<strong>{{ book.DevConsole.ManageCustomExtensions }}</strong>で表示されます。</li>
  <li><strong>{{ book.DevConsole.cek_authorization_url }}</strong>または<strong>{{ book.DevConsole.cek_privacy_policy_url }}</strong>で提供するページが別のドメインから必要なリソースを読み込む場合、<strong>{{ book.DevConsole.cek_domain_list }}</strong>項目に必要なドメインを追加します。</li>
  <li>（任意）アカウント連携の際に発行されるアクセストークンのスコープをあらかじめ定義している場合、<strong>{{ book.DevConsole.cek_scope }}</strong>項目にそのスコープを追加します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_2.png" />
  <li><strong>{{ book.DevConsole.cek_access_token_uri }}</strong>項目に、サービスのアクセストークンを発行できるURIを入力します。現在、<strong>認可グラントタイプはAuthorization code grantのみサポート</strong>しています。</li>
  <li><strong>{{ book.DevConsole.cek_refresh_token_uri }}</strong>項目に、サービスのアクセストークンを更新できるURIを入力します。</li>
  <li>サービスのアクセストークンを取得した後、HTTPリクエストの送信に必要な<strong>{{ book.DevConsole.cek_client_secret }}</strong>を入力します。クライアントシークレットは、<a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">認可サーバーを構築</a>する際に生成した値です。</li>
  <li><strong>{{ book.DevConsole.cek_client_authentication_scheme }}</strong>は、次のうち認可サーバーのインターフェースの実装に適した値を設定します。
    <ul>
      <li><strong>HTTPベーシック認証（推奨）</strong>：サービスのアクセストークンを取得するために、資格情報をヘッダーに入力される場合</li>
      <li><strong>リクエストボディーの資格情報</strong>：サービスのアクセストークンを取得するために、資格情報をボディーに入力される場合</li>
    </ul>
  </li>
</ol>

<div id="RedirectURI" class="note">
  <p><strong>メモ</strong></p>
  <p>アカウント認証を済ませた後、クライアントがリダイレクトするURIは<code>{{ book.ServiceEnv.RedirectURIforAccountLinking }}</code>で、<strong>{{ book.DevConsole.cek_redirect_urls }}</strong>項目で確認できます。</strong><a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">認可サーバー</a>は、この値がクライアントから渡される<code>redirect_uri</code>の値と一致するか検証する必要があります。</p>
  <img src="/DevConsole/Assets/Images/DevConsole-Redirect_URI_for_Extension_Accoun_Linking.png" />
</div>
