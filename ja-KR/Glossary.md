<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: Glossary -->

# 用語および略語

<div class="note">
  <p><strong>メモ</strong></p>
  <p>このページは随時更新されます。</p>
</div>

### CEK
[Clova Extensions Kit](#CEK)の略語

### CIC
[Clova Interface Connect](#CIC)の略語

### Clova {#Clova}
[Clova](https://clova.ai)は、{{ book.ServiceEnv.OrientedService }}が開発およびサービスを提供するAIプラットフォームです。ユーザーの音声やイメージを認識し、それを解析して、ユーザーの希望する情報やサービスを提供します。サードパーティの開発者は、Clovaの持つ技術を活用して、AIサービスを提供するデバイスまたは家電製品を制作できます。また、Clovaを利用して、保有しているコンテンツやサービスを提供することもできます。

### Clova Developer Center {#ClovaDeveloperConsole}
Clovaプラットフォームと連携するクライアントデバイス、または[Clova Extension](#ClovaExtension)を開発する開発者に次の内容を提供する<a target="_blank" href="{{ book.ServiceEnv.DeveloperConsoleURI }}">Webツール</a>です。
* Clova Extensionを[登録](/DevConsole/Guides/Register_Custom_Extension.md)および[配布](/DevConsole/Guides/Deploy_Custom_Extension.md)する
* [対話モデルを登録する](/DevConsole/Guides/Register_Interaction_Model.md)
* Clovaサービスに関する統計資料の提供（今後サービス予定）

### Clova Extension {#ClovaExtension}
音楽、ショッピング、金融などの外部のサービス（サードパーティサービス）、または家庭のIoTデバイスの制御など、Clovaの機能を拡張して、ユーザーに様々な経験を提供するWebアプリケーションです。通常、Extensionと呼ばれます。Clovaプラットフォームは、現在次の2種類のClova Extensionをサポートおよび提供しています。エンドユーザーには、「スキル」という名称で提供されます。
* [Custom Extension](#CustomExtension)
* [Clova Home Extension](#ClovaHomeExtension)

### Clova Extensions Kit（CEK） {#CEK}
Clova Extensionを開発および配布する際に、必要なツールとインターフェースを提供するプラットフォームです。[ClovaとExtensionのコミュニケーション](/Develop/CEK_Overview.md)をサポートします。

### Clova Home Extension {#ClovaHomeExtension}
IoTデバイス制御サービスを提供するための[Extension](#ClovaExtension)です。詳細については、[Clova Home Extensionを作成する]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md)ドキュメントを参照してください。

### Clova Interface Connect（CIC） {#CIC}
AIアシスタントサービスを提供するパソコン/モバイルアプリ、モバイルデバイスまたは家電製品などのクライアントに、Clovaとの連携ができるインターフェースを提供するプラットフォームです。詳細については、[CICの概要]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/CIC_Overview.md)ドキュメントを参照してください。

### Clovaアプリ {#ClovaApp}

{{ book.ServiceEnv.OrientedService }}が開発し、iOSおよびAndroidプラットフォームで配布したClovaアプリです。Clovaに指示を与えるだけでなく、クライアントデバイスを登録し、管理できるアプリです。

### コンテンツテンプレート {#ContentTemplate}
CICから渡されるコンテンツ情報をスケジュールカテゴリに合った形で標準化したものです。詳細については、[content template]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Content_Templates.md)ドキュメントを参照してください。

### Custom Extension {#CustomExtension}
任意の拡張された機能を提供する[Extension](#ClovaExtension)です。Custom Extensionを利用すると、音楽、ショッピング、金融など、外部サービスの機能を提供できます。詳細については、[Custom Extensionを作成する](/Develop/Guides/Build_Custom_Extension.md)ドキュメントを参照してください。

### Custom Extensionのメッセージ {#CustomExtMessage}
[Clova Extensions Kit](#CEK)と[Custom Extension](#CustomExtension)が情報のやり取りをする際に使用するメッセージです。詳細については、[Custom Extensionのメッセージ](/Develop/References/Custom_Extension_Message.md)ドキュメントを参照してください。

### Extension {#Extension}
[Clova Extension](#ClovaExtension)の別の言い方

### Extensionページ {#ExtensionPage}

スキルストアホーム（**スキルの管理**メニュー）で特定のExtensionを選択すると表示されるページです。Extensionに関する詳しい説明が提供されます。

### インテント {#Intent}
Clova Extensionが処理するユーザーの意図を区分したカテゴリです。カスタムインテントとビルトインインテントの2種類があります。[Custom Extension](#CustomExtension)を実装する前に、まずインテントの集合である[対話モデル](#InteractionModel)を定義する必要があります。詳細については、[対話モデルを定義する](/Design/Design_Custom_Extension.md#DefineInteractionModel)ドキュメントを参照してください。

### IntentRequest {#IntentRequest}

ユーザーのリクエストを解析した結果（[インテント](#Intent)）を[Custom Extension](#CustomExtension)に送る際に使用されるリクエストメッセージタイプです。詳細については、[Custom Extensionでリクエストを処理する](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest)ドキュメントを参照してください。

### 対話モデル {#InteractionModel}
[Custom Extension](#CustomExtension)が音声から認識されたユーザーのリクエストをExtensionに送るために、標準化したフォーマット（JSON）に変換するルールを指定したものです。詳細については、[対話モデルを定義する](/Design/Design_Custom_Extension.md#DefineInteractionModel)ドキュメントを参照してください。

### LaunchRequest {#LaunchRequest}
ユーザーが特定のモードまたは特定の[Custom Extension](#CustomExtension)を使用すると宣言したことを知らせるために送るリクエストメッセージです。詳細については、[Custom Extensionでリクエストを処理する](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest)ドキュメントを参照してください。

### OAuth 2.0
アクセス権限を委任するためのオープン標準です。インターネットのユーザーが他のウェブサービスやアプリケーションのユーザーアカウントにアクセスできる権限を付与する規約です。Clovaプラットフォームでは、クライアントが[Clovaアクセストークン](#ClovaAccessToken)を取得したり、ユーザーが特定のExtensionを使用したりする際、自身の[アカウントを連携](/Develop/Guides/Link_User_Account.md)するために使用されます。詳細については、[https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749)を参照してください。

### SessionEndedRequest {#SessionEndedRequest}
ユーザーが特定のモードまたは特定の[Custom Extension](#CustomExtension)の使用を中止すると宣言したことを知らせるために送るリクエストメッセージです。詳細については、[Custom Extensionでリクエストを処理する](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest)ドキュメントを参照してください。

### スキル {#Skill}

Clovaがユーザーに提供する拡張機能やサービスなどをいいます。スキルをユーザーに提供するには、[Clova Extension](#ClovaExtension)を開発する必要があります。

### スキルストア {#SkillStore}

スキルをユーザーに提供するために構築されたプラットフォームです。

### スキルストアホーム {#SkillStoreHome}

スキルストアに登録されたスキルが表示されるページです。Clovaアプリの**スキルの管理**メニューを指す用語です。

### スロット {#Slot}
[インテント](#Intent)に宣言されたリクエストを処理する際に必要な情報です。インテントを定義するとき、共に定義する必要があります。Clovaはユーザーのリクエストを解析して、スロットに該当する情報を抽出します。詳細については、[対話モデルを定義する](/Design/Design_Custom_Extension.md#DefineInteractionModel)ドキュメントを参照してください。

### アカウントを連携する（Account Linking） {#AccountLinking}
[Extension](#ClovaExtension)がユーザーのアカウント認証を必要とする外部サービスを提供する際に使用されます。詳細については、[ユーザーアカウントを連携する](/Develop/Guides/Link_User_Account.md)ドキュメントを参照してください。

### ユーザーのサンプル発話 {#UserUtteranceExample}

ユーザーのリクエスト発話がどのように入力されるかを例で表現したリストです。[インテント](#Intent)ごとに複数の例を定義できます。また、例には[スロット](#Slot)が表示されます。詳細については、[対話モデルを定義する](/Design/Design_Custom_Extension.md#DefineInteractionModel)ドキュメントを参照してください。

### セッションID {#SessionID}
[Extension](#ClovaExtension)がユーザーリクエストのコンテキストを区分するためのセッション識別子です。通常、一回性のユーザーリクエストは、リクエスト毎にセッションIDが異なりますが、特定のモードや連続的な（マルチターン）ユーザーリクエストの場合、同じセッションIDを持ちます。このセッションIDは、[Clova Extensions Kit](#CEK)がExtensionにユーザーのリクエストを渡すとき生成されます。セッションIDが維持されるのは、[LaunchRequest](#LaunchRequest)のようなリクエストを受け取ったり、またはExtensionが必要に応じて`response.shouldEndSession`フィールドを`false`に設定した場合です。詳細については、[Custom Extensionを作成する](/Develop/Guides/Build_Custom_Extension.md)ドキュメントを参照してください。

<!-- End of the shared content -->
