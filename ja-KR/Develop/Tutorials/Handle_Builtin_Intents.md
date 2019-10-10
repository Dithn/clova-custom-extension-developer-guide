# 基本的な意思表現を処理する
このチュートリアルでは、[簡単なExtensionを作成する](/Develop/Tutorials/Build_Simple_Extension.md)で作成したサンプルサイコロExtensionを利用して、「はい」「いいえ」などの基本的な意思の表現を処理する方法について説明します。

Clovaは、頻繁に発生するユーザーの基本的な意思表現をすべてのExtensionで共通で使用できるように、[ビルトインインテント](/Design/Design_Custom_Extension.md#BuiltinIntent)としてあらかじめ定義しています。以下は、Clovaが提供するビルトインインテントの一覧です。

| ビルトインインテントの名前       | 意図               | ユーザーのサンプル発話                                      |
|---------------------------|-------------------|----------------------------------------------------------|
| Clova.GuideIntent         | ヘルプのリクエスト          | 使い方を教えて |
| Clova.CancelIntent        | 実行キャンセルのリクエスト        | キャンセル                                          |
| Clova.YesIntent           | 肯定の返事（はい、Yes）   | はい                   |
| Clova.NoIntent            | 否定の返事（いいえ、No） | いいえ                                     |

ユーザーがヘルプや実行のキャンセルをリクエストするとき、そのリクエストを処理するために、上記の表にあるビルトインインテントを使用することができます。1番目のチュートリアルで行なったようにインテントとサンプル発話を登録する必要はありません。

ビルトインインテントを処理するためには、次の作業が必要です。
* ステップ1ビルトインインテントの処理を実装する（Extensionサーバーで作業）
* ステップ2ビルトインインテントの動作をテストする（Clova Developer Centerで作業）

## ステップ1ビルトインインテントの処理を実装する {#Step1}

サンプルサイコロExtensionがビルトインインテントを処理できるように、コードを変更する必要があります。

ここでは、基本的な処理方法を説明するために、「ヘルプ」ビルトインインテントのみ扱います。
「ヘルプ」ビルトインインテントは、ユーザーが「使い方を教えて」「ヘルプをお願い」のようなフレーズを発する際に送信され、`Clova.GuideIntent`という名前を使用します。

このビルトインインテントを処理するために、以下のように[1番目のチュートリアル](/Develop/Tutorials/Build_Simple_Extension.md)のリポジトリを再使用します。
次のようにローカルリポジトリにアクセスして、`tutorial2`ブランチに切り替えます。

```bash
cd /path/to/your_sample_dice_directory
git fetch
git checkout tutorial2
```

サンプルサイコロExtensionは、`clova/index.js`ファイルの`intentRequest()`でヘルプのリクエストを処理します。

```javascript
intentRequest(cekResponse) {
  const intent = this.request.intent.name

  switch (intent) {
    ...
    case "Clova.GuideIntent":
    default:
      cekResponse.setSimpleSpeechText(「サイコロを1つ振って、と言ってみてください」）
  }
  ...
}
```

上記のコードのように、Clovaからのリクエストメッセージからインテントを抽出し、その名前が`Clova.GuideIntent`の場合、「サイコロを1つ振って」と話すように案内します。

変更されたコードをExtensionサーバーで実行します。

## ステップ2ビルトインインテントの動作をテストする {#Step2}
サンプルサイコロExtensionがヘルプをリクエストするビルトインインテントを正しく処理するかテストする必要があります。

[1番目のチュートリアル](/Develop/Tutorials/Build_Simple_Extension.md)のように、2つのテスト方法があります。1つは、Clova Developer Centerで対話モデルの動作を確認する方法です。もう1つは、テスターIDを登録して、Clovaアプリで実際の動作を確認する方法です。
このチュートリアルでは、対話モデルの動作のみ確認します。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>ビルトインインテントは、対話モデルに明示的に登録しなくても動作します。今後、Extensionごとに必要なビルトインインテントのみ選択して登録できるように変更される予定です。</p>
</div>

次の順で、サンプルサイコロExtensionがヘルプのリクエストを正しく処理するか確認します。

1. <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>に接続します。
2. サンプルサイコロの**{{ book.DevConsole.cek_interaction_model }}**項目の**{{ book.DevConsole.cek_edit}}**ボタンをクリックします。
3. 画面左上の**{{ book.DevConsole.cek_builder_menu_build }}**ボタンをクリックして、対話モデルをビルドします。
4. ビルドが終わったら、左側のメニューリストで**{{ book.DevConsole.cek_test }}**メニューをクリックします。
5. **{{ book.DevConsole.cek_builder_test_expression_title }}**に、ヘルプを求めるフレーズを入力します。例えば、「使い方を教えて」と入力します。
6. Enterキーを押すか、または**{{ book.DevConsole.cek_builder_test_request_test }}**ボタンをクリックします。
7. **{{ book.DevConsole.cek_builder_test_result_title }}**の**{{ book.DevConsole.cek_builder_test_intent_result }}**項目に「Clova.GuideIntent」と表示されるか確認します。<br />
	![](/Develop/Assets/Images/CEK_Tutorial_Builtin_Intent_Test.png)
  <div class="note">
  	<p><strong>メモ</strong></p>
  	<p>外部からアクセスできるExtensionサーバーのURIを登録していない場合、<strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>は「{{ book.DevConsole.cek_builder_test_no_response }}」と表示されます。</p>
	</div>

こうすることで、サンプルサイコロExtensionが、ヘルプのリクエストに応答できるようになります。
このようにExtensionサーバーが`Clova.CancelIntent`、`Clova.YesIntent`、`Clova.NoIntent`を処理するように実装すると、実行のキャンセル、肯定または否定を表すリクエストにも応答できます。
