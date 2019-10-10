# このドキュメントについて
このドキュメントは、Clova Custom Extensionの開発をサポートするために、デザインガイドラインとCEKプラットフォームの開発ガイドおよびAPIリファレンス、そしてClova Developer Centerについてのガイドを提供します。対象となる読者は、CEKを使用したオンラインコンテンツおよびサービスの提供を希望するExtension開発者です。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Clovaは、現在も開発が進められています。このドキュメントの内容は随時変更される可能性があります。</p>
</div>

## お問い合わせ
ドキュメントの内容に関するお問い合わせは、指定のClova提携担当者、または<a href="{{ book.ServiceEnv.DeveloperCenterForumURI }}" target="_blank">{{ book.ServiceEnv.DeveloperCenterName }}フォーラム</a>までお願いします。

## ドキュメントの変更履歴

このドキュメントの変更履歴は、以下のとおりです。

<table>
  <thead>
    <tr>
      <th style="width:12%">リリース日</th><th style="width:88%">変更履歴</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019/08/19</td>
      <td>
        <ul>
          <li>APIリファレンスとDeveloper Centerガイドの目次レベルを1つずつ上げる</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/08/05</td>
      <td>
        <ul>
          <li>Custom Extensionのデザインガイドラインドキュメントを<a href="/Design/Design_Custom_Extension.md">Custom Extensionをデザインする</a>、<a href="/Design/Supported_Audio_Format.md">プラットフォームでサポートされる音声ファイルフォーマット</a>、<a href="/Design/Rules_For_Content.md">コンテンツガイドライン</a>ページに分割</li>
          <li><a href="/Design/Supported_Audio_Format.md">プラットフォームでサポートされる音声ファイルフォーマット</a>に、Clovaでサポートされるコンテナフォーマットを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/07/31</td>
      <td>
        <ul>
          <li>ClovaデベロッパーガイドからClova Custom Extensionガイドに分割</li>
          <li>一部のリンクの誤りおよび誤字・脱字を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/07/02</td>
      <td>
        <ul>
          <li><a href="/DevConsole/Guides/Register_Interaction_Model.md">対話モデルを登録する</a>で、サンプル発話の登録およびTSVファイルのアップロードに関する制約を説明に追加</li>
          <li>一部のサンプルの誤字・脱字を修正およびメモボックスのレベルを調整</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/06/10</td>
      <td>
        <ul>
          <li>新規コンテンツ再生のために、<a href="/Design/Design_Custom_Extension.md">Custom Extensionをデザインする</a>の<a href="/Design/Design_Custom_Extension.md#BuiltinIntent">ビルトインインテント</a>にClova.RequestAlternativesIntentを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/05/20</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Custom_Extension_Message.html#CustomExtSpeechInfoObject">SpeechInfoObject</a>のtoken、valueフィールド</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/04/01</td>
      <td>
        <ul>
          <li>Extensionがクライアントから合成音声（TTS）の再生状態のレポートを受信できるように、<a href="/Develop/References/Custom_Extension_Message.html#CustomExtSpeechInfoObject">SpeechInfoObject</a>にtokenフィールドを追加し、<a href="/Develop/Guides/Monitor_TTS_Playback_Status.md">音声の再生状態を確認する</a>ガイドドキュメントを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/03/25</td>
      <td>
        <ul>
          <li>HLSストリームを提供するために、contentTypeフィールドをCustom Extensionのメッセージの<a href="/Develop/References/Custom_Extension_Message.html#CustomExtSpeechInfoObject">SpeechInfoObject</a>に追加</li>
          <li>一部リンクの誤りおよびサンプルを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/03/13</td>
      <td>
        <ul>
          <li>一部のリファレンスドキュメントの内容順序を修正</li>
          <li>内部・外部からのフィードバックをドキュメントに反映</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/02/26</td>
      <td>
        <ul>
          <li>Extensionという表現がCustom ExtensionとClova Home Extensionの両方を含めるため、ドキュメント内でExtensionとなっていた個所を具体的に明記</li>
          <li>UI要素を削除した個所に対し、URLの表現をURIに変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/01/25</td>
      <td>
        <ul>
          <li>ダイアグラムの一部のスタイルを修正し、表の列間隔を調整</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/01/07</td>
      <td>
        <ul>
          <li>ドキュメントで使用されている一部のUMLダイアグラムのスタイルを統一</li>
          <li>ドキュメントの変更履歴のうち、一部表記の誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/12/24</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Custom_Extension.md">Custom Extensionをデザインする</a>ドキュメントのオーディオコンテンツの再生タイプ説明のうち、サンプルのシナリオで正しくないディレクティブ名を訂正</li>
          <li><a href="/Develop/Guides/Provide_Audio_Content.md">オーディオコンテンツを提供する</a>で、CIC仕様の説明を読者が分かりやすい表現に修正</li>
          <li>一部のシーケンスダイアグラムで、正しく表記されてないノード型を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/12/22</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Link_User_Account.md">ユーザーアカウントを連携する</a>ガイドの<a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">認可サーバーを構築する</a>で、redirect_uriパラメータのうち、誤りがあったvendorIdフィールドを削除</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/10/13</td>
      <td>
        <ul>
          <li><a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">Extensionの基本情報を入力する</a>に呼び出し名を1つから3つまで登録できることを明記</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/09/21</td>
      <td>
        <ul>
          <li>Clovaから送信されるメッセージの検証のために、<a href="/Develop/References/HTTP_Message.md#HTTPHeader">HTTPヘッダー</a>にSignatureCEKフィールドの説明を追加し、<a href="/Develop/Guides/Build_Custom_Extension.md">Custom Extensionを作成する</a>ドキュメントに、リクエストメッセージを検証するセクションを追加</li>
          <li>誤りがあった一部のサンプールコードを訂正</li>
          <li>誤りがあった一部のリンクを訂正</li>
          <li>ユーザータッチポイントにある一部のExtensionの表記をスキルに変更（UI画像を一緒に更新）</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/09/07</td>
      <td>
        <ul>
          <li>デザインガイドラインの<a href="/Design/Supported_Audio_Format.md">プラットフォームでサポートされる音声ファイルフォーマット</a>に、オーディオコンテンツごとのオーディオ属性とラウドネスに関する推奨事項を追加</li>
          <li>サンプルの説明で、「yourdomain.com」になっているサンプルをドキュメント作成用のドメイン名である「example.com」に変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/08/24</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Custom_Extension_Message.md">Custom Extensionのメッセージ</a>の<a href="/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest">EventRequest</a>リクエストタイプのサンプルで誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/08/09</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Build_Custom_Extension.md#ProvidingMetaDataForDisplay">オーディオコンテンツのメタデータを提供する</a>セクションで一部の誤字・脱字を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/07/09</td>
      <td>
        <ul>
          <li>Custom Extensionの<a href="/Design/Design_Custom_Extension.md#DefineInvocationName">名前を定義する</a>のガイドラインを追加</li>
          <li>Custom Extensionの<a href="/Design/Rules_For_Content.md">コンテンツガイドライン</a>を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/06/25</td>
      <td>
        <ul>
          <li>Custom Extensionの<a href="/Design/Design_Custom_Extension.md#DecideSoundOutputType">応答タイプ</a>のガイドラインを追加</li>
          <li>Custom Extensionを作成するドキュメントに<a href="/Develop/Guides/Provide_Audio_Content.md">オーディオコンテンツを提供する</a>セクションを追加</li>
          <li>Custom Extensionのメッセージの<a href="/Develop/References/Custom_Extension_Message.md#CustomExtRequestType">リクエストタイプ</a>に<a href="/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest">EventRequestタイプ</a>を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/05/14</td>
      <td>
        <ul>
          <li>HTTPリクエストメッセージにヘッダー（SignatureCEK、SignatureCEKCertChainUrl）、およびリクエストメッセージを検証するセクションを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/04/02</td>
      <td>
        <ul>
          <li>Clova Developer Centerの一部のUIを更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/03/05</td>
      <td>
        <ul>
          <li><a href="/Develop/Tutorials/Introduction.md">チュートリアル</a>ページに<a href="/Develop/Tutorials/Use_Builtin_Type_Slots.md">ユーザーから入力された情報を活用する</a>ページを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/02/05</td>
      <td>
        <ul>
          <li>Extensionを開始する呼び出し（<a href="/Develop/Guides/Build_Custom_Extension.md#HandleLaunchRequest">LaunchRequest</a>）の説明を修正および<a href="/Design/Design_Custom_Extension.md">Custom Extensionをデザインする</a>ドキュメントに反映</li>
          <li>CEKとExtensionの通信に使用される<a href="/Develop/CEK_Overview.md#WhatisCEK">HTTPプロトコルのバージョン</a>を明記</li>
          <li><a href="/Develop/Tutorials/Introduction.md">チュートリアル</a>ページに<a href="/Develop/Tutorials/Handle_Builtin_Intents.md">基本的な意思表現を処理する</a>ページを追加</li>
          <li>Extensionサーバーで使用すべき<a href="/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection">ポート</a>を明記</li>
          <li>一部のドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/29</td>
      <td>
        <ul>
          <li><a href="/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection">Extensionサーバーとの連携を設定する</a>前に接続を確認する方法および<a href="/DevConsole/Guides/Test_Custom_Extension.md#TestOnClovaApp">テスターID適用の自動化</a>の案内を追加</li>
          <li>Clova Developer Centerの一部のUIを更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/22</td>
      <td>
        <ul>
          <li><a href="/Design/Supported_Audio_Format.md">プラットフォームでサポートされる音声ファイルフォーマット</a>をデザインガイドラインに追加</li>
          <li><a href="/Develop/Tutorials/Introduction.md">チュートリアル</a>ページと<a href="/Develop/Tutorials/Build_Simple_Extension.md">簡単なExtensionを作成する</a>ページを追加</li>
          <li><a href="/DevConsole/Guides/Register_Interaction_Model.md#AddCustomSlotType">ビルトインインテントリストの表示</a>、<a href="/DevConsole/Guides/Deploy_Custom_Extension.md#InputComplianceInfo">審査をリクエストする</a>際に、コメントを作成するUIを追加</li>
          <li>UMLダイアグラムの画像形式を変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/08</td>
      <td>
        <ul>
          <li>プラットフォームの実装状況に合わせて<a href="/Design/Design_Custom_Extension.md#DefineInteractionModel">ビルトインインテント</a>の説明を修正</li>
          <li><a href="/Develop/Examples/Extension_Examples.md">サンプルのExtension</a>ページを追加</li>
          <li><strong>テスターID</strong>フィールドの追加による<a href="/DevConsole/Guides/Test_Custom_Extension.md">Extensionをテストする</a>の説明を更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/02</td>
      <td>
        <ul>
          <li>一部のドキュメントの誤りと誤字を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/12/18</td>
      <td>
        <ul>
          <li><a href="/DevConsole/Guides/Register_Interaction_Model.md">対話モデルを登録する</a>で、<a href="/Design/Design_Custom_Extension.md#DefineInteractionModel">対話モデルを定義する</a>セクションの内容を<a href="/Design/Design_Custom_Extension.md">Custom Extensionをデザインする</a>ドキュメントに移動</li>
          <li><a href="/Design/Design_Custom_Extension.md#DefineInteractionModel">対話モデルを定義する</a>セクションの内容に、<a href="/Design/Design_Custom_Extension.md#UtteranceExample">サンプル発話</a>フレーズの作成ガイドラインを追加</li>
          <li><a href="/DevConsole/Guides/Test_Custom_Extension.md">Extensionをテストする</a>にテストモードを使用するを追加</li>
          <li>UIの改善による画像および説明を修正</li>
          <li><a href="/DevConsole/Guides/Update_Custom_Extension.md">Extensionをアップデートする</a>、<a href="/DevConsole/Guides/Remove_Custom_Extension.md">Extensionを中止・削除する</a>を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/12/11</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Custom_Extension.md">Custom Extensionをデザインする</a>を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/12/04</td>
      <td>
        <ul>
          <li>ユーザーのマルチターン対話のために、repromptフィールドを<a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">レスポンスメッセージ</a>に追加</li>
          <li>一部のドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/11/06</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Custom_Extension_Message.md">Custom Extensionのメッセージ</a>のうち、リクエストメッセージのcontext.System.device.displayTypeフィールドの名前をcontext.System.device.displayに変更し、サブフィールドの構成を変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/10/30</td>
      <td>
        <ul>
          <li><a href="/DevConsole/ClovaDevConsole_Overview.md">Clova Developer Centerの概要</a>説明を追加</li>
          <li><a href="/DevConsole/Guides/Register_Custom_Extension.md">Extensionを登録する</a>ガイドを追加</li>
          <li><a href="/DevConsole/Guides/Register_Interaction_Model.md">対話モデルを登録する</a>ガイドを追加</li>
          <li><a href="/DevConsole/Guides/Deploy_Custom_Extension.md">Extensionを配布する</a>ガイドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/10/23</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Custom_Extension_Message.md">Custom Extensionのメッセージ</a>のうち、リクエストメッセージにcontext.System.device.displayTypeフィールドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/10/16</td>
      <td>
        <ul>
          <li>一部のドキュメントで画像を修正およびドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/09/04</td>
      <td>
        <ul>
          <li>一部のドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/08/14</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Do_Multiturn_Dialog.md">マルチターン対話をする</a>セクションを追加およびsessionAttributesフィールドの説明を更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/07/07</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">Custom Extensionレスポンスメッセージ</a>の<a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">outputSpeech</a>オブジェクト構成の更新を反映</li>
          <li><a href="/Glossary.md">用語集を追加</a></li>
          <li>CEKのメッセージフォーマット部分の目次を更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/07/03</td>
      <td>
        <ul>
          <li>CEKドキュメントの画像を更新</li>
          <li>ドキュメントの検討結果を反映</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/06/19</td>
      <td>
        <ul>
          <li>CEKドキュメントを作成</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
