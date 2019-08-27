# Custom Extensionを作成する

Custom Extensionとは、Clovaが基本的に提供している機能やサービスではなく、開発者が任意に拡張した機能や、外部のサービスを提供するExtensionです。例えば、ウェブ検索やニュースクリッピングなどのサービスだけでなく、ユーザーアカウント認証の必要な音楽、ショッピング、金融などの外部サービスを提供することができます。Custom Extensionは、解析されたユーザーの発話情報をCEKから受け取り、その内容を処理して、サービスの処理結果を返す必要があります。

このドキュメントでは、Custom Extensionを作成する際の準備事項と、Custom ExtensionがCEKとどのようなメッセージのやり取りをして、どのように動作する必要があるかについて、次の内容を説明します。

* [準備事項](#Preparation)
* [Custom Extensionリクエストを処理する](#HandleCustomExtensionRequest)
   * [`LaunchRequest`リクエストを処理する](#HandleLaunchRequest)
   * [`IntentRequest`リクエストを処理する](#HandleIntentRequest)
   * [`SessionEndedRequest`リクエストを処理する](#HandleSessionEndedRequest)
   * [リクエストメッセージを検証する](#RequestMessageValidation)
* [Custom Extensionでレスポンスを返す](#ReturnCustomExtensionResponse)

{% include "/Develop/Guides/BuildCustomExtension/Preparation.md" %}

{% include "/Develop/Guides/BuildCustomExtension/Handle_Custom_Extension_Request.md" %}

{% include "/Develop/Guides/BuildCustomExtension/Validating_Request_Message.md" %}

{% include "/Develop/Guides/BuildCustomExtension/Return_Custom_Extension_Response.md" %}