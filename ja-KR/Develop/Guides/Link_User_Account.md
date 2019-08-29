<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: LinkUserAccount -->

# ユーザーアカウントを連携する
Clovaは、[Custom Extension](/Develop/Guides/Build_Custom_Extension.md)、または[Clova Home Extension]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md)を介して、ユーザーアカウント権限を必要とする外部サービスを提供することができます。例えば、有料コンテンツサービスの音楽ストリーミングサービスや、ショッピング、金融、ホームIoTなどのサービスをClovaと連携することができます。そのために、Clovaは外部サービスのユーザーアカウントとClovaのユーザーアカウントを関連付けるアカウント連携（account linking）をサポートしています。アカウントの連携は、[OAuth 2.0](https://tools.ietf.org/html/rfc6749)を使用して行われます。

アカウント連携は、Custom Extensionがユーザーのアカウント認証を必要とする外部サービスを提供する際に使用されます。アカウント認証を必要としない外部サービスはアカウント連携なしに提供できます。ユーザーを識別できる程度の情報を必要とするサービスは、通常、[Custom Extensionのメッセージ](/Develop/References/Custom_Extension_Message.md)が提供する端末識別子（`context.System.device.deviceId`）とユーザーアカウント識別子（`context.System.user.userId`または`session.user.userId`）を組み合わせた値を使用します。

<div class="note">
<p><strong>メモ</strong></p>
<p>Clova Home Extensionは、必ずアカウント連携を使用する必要があります。</p>
</div>

このドキュメントでは、次の内容について説明します。
* [アカウント連携について](#UnderstandAccountLinking)
* [アカウント連携を適用する](#ApplyAccountLinking)

## アカウント連携について {#UnderstandAccountLinking}
アカウント連携をExtensionに適用するためには、アカウント連携の仕組みについて知る必要があります。ここでは次の内容を説明します。
* [アカウント連携を設定する](#SetupAccountLinking)
* [アカウント連携後にExtensionを呼び出す](#ExtensionInvokingAfterAccountLinking)

### アカウント連携を設定する {#SetupAccountLinking}
ユーザーがアカウント認証の必要なCustom ExtensionまたはClova Home Extensionをアクティブにすると、次のようにアカウント連携の設定が開始されます。

![](/Develop/Assets/Images/CEK_Account_Linking_Setup_Sequence_Diagram.svg)

1. ユーザーが特定のCustom ExtensionまたはClova Home Extensionを有効にします。

2. Clovaアプリまたはクライアントデバイスとペアリングするアプリで、外部サービスのログインページを表示します。その際、Extensionの開発者があらかじめ登録した認可サーバーの[認証URI](#BuildAuthServer)を使用します。

3. ユーザーがアカウント認証を完了すると、認可コードまたはアクセストークンが返されます。

4. 渡された認可コードまたはアクセストークンがリダイレクトURLからClovaに渡されます。

5. Clovaは、[アクセストークンURI](#RegisterAccountLinkingInfo)にアクセストークンとリフレッシュトークンをリクエストします。その際、認可コードを渡し、取得したアクセストークンとリフレッシュトークンをユーザーのClovaアカウントに保存します。

6. ユーザーはアカウント認証の必要なサービスを使用できるようになります。

<div class="note">
<p><strong>メモ</strong></p>
<p>ユーザーが特定のCustom ExtensionまたはClova Home Extensionを無効にした場合、ユーザーのClovaアカウントに保存されていたアクセストークンは削除されます。その後、ユーザーがそのExtensionを再びアクティブにすると、再度アカウント連携を行う必要があります。</p>
</div>

### アカウント連携後にExtensionを呼び出す {#ExtensionInvokingAfterAccountLinking}
アカウント連携が完了した状態で、CEKは次の順序でExtensionを呼び出します。

1. ユーザーのリクエストを処理するために、通常どおりにExtensionを呼び出します。

2. **（アクセストークンが期限切れの場合）**リフレッシュトークンを使用して[アクセストークンURI](#RegisterAccountLinkingInfo)に新しいアクセストークンをリクエストします。

3. Extensionに渡すメッセージにアクセストークンを含めてユーザーのリクエストを渡します。
   * Custom Extensionは、`context.System.user.accessToken`と`session.user.accessToken`フィールドにアクセストークンが含まれます。
   * Clova Home Extensionは、`payload.accessToken`フィールドにアクセストークンが含まれます。

4. Extensionは、状況に応じて次のように応答する必要があります。
   * アクセストークンが有効の場合、ユーザーのリクエストを処理し、その結果を返す必要があります。
   * アクセストークンが無効の場合、[アカウント連携の設定](#SetupAccountLinking)に進むように結果を返す必要があります。

## アカウント連携を適用する {#ApplyAccountLinking}
開発しているExtensionにアカウント連携を適用するには、以下の項目を行う必要があります。

1. [認可サーバーを構築する](#BuildAuthServer)
2. [アカウント権限の検証を実装する](#AddValidationLogic)
3. [アカウント連携情報を登録する](#RegisterAccountLinkingInfo)

### 認可サーバーを構築する {#BuildAuthServer}

Extensionにアカウント連携を適用するには、ユーザーがアカウント認証を行うログインページと、認証後にアクセストークンを発行するサーバーを構築する必要があります。

ユーザー認証のために提供するログインページは、次の項目を満足させる必要があります。
* **HTTPSプロトコル**でページを提供する必要があります。そのとき、**443ポート**を使用しなければなりません。
* モバイル用のページをサポートします。
* ポップアップウィンドウを提供しません。
* 認証が完了すると、リダイレクトURI（`redirect_uri`）に遷移します。その際、認可コードをパラメータで送信する必要があります。
* `state`パラメータをリダイレクトURI（`redirect_uri`）に引き続き送信します。

ユーザーがアカウントを認証できるようにログインUIを提供するページのアドレスを**認証URI**と呼びます。認証URIは、Clova Developer CenterでExtensionを登録するときに入力します。ユーザーがExtensionのアカウント連携を使用するように設定（[Custom Extensionの場合](/DevConsole/Guides/Register_Custom_Extension.md#SetAccountLinking)、Clova Home Extensionの場合）する際、この**認証URI**が次のパラメータと一緒に呼び出されます。

| パラメータ名     | 説明                                                       |
|----------------|-----------------------------------------------------------|
| `state`         | 認証セッションの期限切れを確認するステータス値この値は5分後に期限切れとなるため、ユーザーが5分以内に認証を済まさない場合、再度認証を行う必要があります。                                                                                                                                                   |
| `client_id`     | Clovaが外部サービスのアクセストークンを取得するために使用するID開発者は、Clova Developer Centerであらかじめ`cliend_id`を登録しておく必要があります。                                                                                                                                                     |
| `response_type` | OAuth 2.0認可グラントタイプを定義したパラメータ。`"code"`タイプを使用します。Clova Developer Centerで指定します。現在、`"code"`タイプのみサポートしています。              |
| `scope`         | OAuthの`scope`フィールドアクセスレベルを定義できます。Clova Developer Centerであらかじめ`scope`を登録しておく必要があります。                                                                                                                                                                           |
| `redirect_uri`  | アカウント認証後にリダイレクトするURIです。`redirect_uri`の値は、Clova Developer CenterでExtensionを登録するときに、アカウント連携の設定（[Custom Extensionの場合](/DevConsole/Guides/Register_Custom_Extension.md#SetAccountLinking)、Clova Home Extensionの場合）から確認できます。現在、`{{ book.ServiceEnv.RedirectURIforAccountLinking }}`が使用されています。 |

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>パラメータの詳細については、OAuth 2.0 Authorization Frameworkの<a href="https://tools.ietf.org/html/rfc6749#section-4">Obtaining Authorization</a>を参照してください。</p>
</div>

以下はクライアントアプリまたはクライアントデバイスとペアリングするアプリが、ログインページをリクエストするURIの例です。

<pre><code>https://example.com/login?state=qwer123
                            &client_id=clova-extension
                            &scope=listen_music%20basic_profile
                            &response_type=code
                            &redirect_uri={{ book.ServiceEnv.RedirectURIforAccountLinking }}
</code></pre>


<div class="Note">
<p><strong>メモ</strong></p>
<p><code>redirect_uri</code>は、Clova Developer Centerのアカウント連携を設定する画面（<a href="/DevConsole/Guides/Register_Custom_Extension.md#RedirectURI">Custom Extensionの場合</a>、Clova Home Extensionの場合）から確認できます。クライアントから受け取る<code>redirect_uri</code>の値が、Clovaから渡されるリダイレクトURIと一致するか<a href="https://tools.ietf.org/html/rfc6749#section-10.6" target="_blank">検証</a>する必要があります。</p>
</div>

アカウント連携後にリダイレクトするURI（`redirect_uri`）に、次のパラメータを渡す必要があります。

| パラメーター     | 説明                                                       |
|----------------|-----------------------------------------------------------|
| `state`        | 認証セッションの期限切れを確認するステータス値**認証URI**から渡された`state`パラメータをそのまま入力します。                                |
| `code`         | 認可コード`response_type`の値が`"code"`の場合、このパラメータに認可コードを入力します。                                                     |
| `token_type`   | アクセストークンのタイプ`access_token`と一緒に渡される必要があります。`"Bearer"`で固定されます。                                                                        |

次はユーザーのアカウント認証完了後、移動するリダイレクトURIの例です。

<pre><code>{{ book.ServiceEnv.RedirectURIforAccountLinking }}?&state=qwer123
                                &code=nl__eCSTdsdlkjfweyuxXvnl
</code></pre>


Clovaはユーザーアカウントを連携するために認可コードを取得すると、Extensionの開発者がClova Developer Centerにあらかじめ登録した[アクセストークンURI](#RegisterAccountLinkingInfo)にアクセストークンをリクエストします。その際、Clovaは取得した認可コードをパラメータに送信します。認可サーバーは外部サービスのアカウント権限が付与されたアクセストークンと、アクセストークンを更新できるリフレッシュトークンを発行する必要があります。

### アカウント権限の検証を実装する {#AddValidationLogic}
アカウント連携を適用するために、Extensionの開発者はアクセストークンの有効性を検証するコードを作成する必要があります。Custom ExtensionとClova Home Extensionに渡されるExtensionメッセージは、それぞれ次のような`accessToken`フィールドを持っています。
以下のフィールドでアクセストークンを確認し、そのアクセストークンが存在していて、有効な値であるか確認する必要があります。

* Custom Extension: `context.System.user.accessToken`, `session.user.accessToken`
* Clova Home Extension: `payload.accessToken`

{% raw %}
```json
//サンプル1：Custom Extensionのメッセージのサンプル
{
  "version": "0.1.0",
  "session": {
    "new": false,
    "sessionAttributes": {},
    "sessionId": "a29cfead-c5ba-474d-8745-6c1a6625f0c5",
    "user": {
      "userId": "V0qe",
      "accessToken": "XHapQasdfsdfFsdfasdflQQ7"
    }
  },
  "context": {
    "System": {
      "application": {
        "applicationId": "com.example.extension.pizzabot"
      },
      "user": {
        "userId": "V0qe",
        "accessToken": "XHapQasdfsdfFsdfasdflQQ7"
      },
      "device": {
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b",
        "display": {
          "size": "l100",
          "orientation": "landscape",
          "dpi": 96,
          "contentLayer": {
            "width": 640,
            "height": 360
          }
        }
      }
    }
  },
  "request": {
    "type": "IntentRequest",
    "intent": {
      "name": "OrderPizza",
      "slots": {
        "pizzaType": {
          "name": "pizzaType",
          "value": "ペパロニ"
        }
      }
    }
  }
}

//サンプル2：Clova Home Extensionメッセージの例
{
  "header": {
    "messageId": "33da6561-0149-4532-a30b-e0de8f75c4cf",
    "name": "HealthCheckRequest",
    "namespace": "ClovaHome",
    "payloadVersion": "1.0"
  },
  "payload": {
    "accessToken": "92ebcb67fe33"
  }
}
```
{% endraw %}

<div class="note">
  <p><strong>メモ</strong></p>
  <p>アクセストークンが存在しないか、または無効の場合、Extensionはクライアントがユーザーアカウントを再度連携するように、CEKに応答を返す必要があります。</p>
</div>


### アカウント連携情報を登録する {#RegisterAccountLinkingInfo}
認可サーバーの構築とExtensionのアカウント連携が完了すると、[Clova Developer Center](/DevConsole/ClovaDevConsole_Overview.md)に、[認可サーバーを構築する](#BuildAuthServer)で説明されている情報を登録する必要があります。Clova Developer Centerに登録されているExtensionで、以下のようなアカウント連携情報を入力（[Custom Extensionの場合](/DevConsole/Guides/Register_Custom_Extension.md#SetAccountLinking)、Clova Home Extensionの場合）します。

| パラメーター     | 説明                                                       |
|----------------|-----------------------------------------------------------|
| 認証URI            | ユーザーが[アカウントを認証](#SetupAccountLinking)するためにアクセスするURI                                      |
| Client ID                    | ユーザー[アカウント認証](#SetupAccountLinking)ページをリクエストする際に、サービスを識別するために付与したクライアントID      |
| Authorization Grant Type     | OAuth 2.0の認可グラントタイプ。現在、Authorization code grantタイプのみサポートしています。                       |
| Access Token URI             | 認可コードでアクセストークンを取得するためのアドレス。                                         |
| Client Secret                | 認可コードでアクセストークンを取得する際に、**Client ID**と一緒に渡されるクライアントシークレットのこと。 |
| Client Authentication Scheme | アクセストークンURIにアクセストークンをリクエストする際に使用されるスキーム                                     |
| プライバシーポリシーURI           | サービスのプライバシーポリシーが提供されるページClovaアプリまたはペアリングアプリに表示されます。       |

<!-- End of the shared content -->
