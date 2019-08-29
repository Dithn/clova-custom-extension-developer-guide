<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Custom Extensionをテストする
登録したCustom Extensionや対話モデルは、配布する前にテストできます。次の項目を確認してCustom Extensionと対話モデルをテストします。

* （Custom Extension専用）[対話モデルをビルドする](#BuildInteractionModel)
* （Custom Extension専用）[対話モデルをテストする](#TestInteractionModel)
* [ClovaアプリでCustom Extensionをテストする](#TestOnClovaApp)

## 対話モデルをビルドする {#BuildInteractionModel}

Custom Extensionを配布する場合、先に[対話モデルを登録](/DevConsole/Guides/Register_Interaction_Model.md)しておく必要があります。また、対話モデルの[テスト](#TestInteractionModel)や実行には、ビルドが必要です。作成した対話モデルは次のようにビルドすることができます。

<ol>
  <li>登録したCustom Extensionのリストから、対話モデルをビルドするCustom Extensionの<strong>{{ book.DevConsole.cek_edit }}</strong>メニューをクリックします。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Interaction_Model_Menu.png" />
  <li><strong>{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.cek_builder_header_title_dashboard }}</strong>画面の左上にある<strong>{{ book.DevConsole.cek_builder_menu_build }}</strong>ボタンをクリックすると、対話モデルのビルドが開始されます。対話モデルのサイズなどによって、数分ぐらいかかることがあります。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Build_Interaction_Model.png" />
</ol>

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>ビルド中に<strong>{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.cek_builder_header_title_dashboard }}</strong>内で他のメニューに移動しても、ビルドはキャンセルされません。ビルドが開始されてからも、自由にメニューを移動したり、内容を編集したりできます。</p>
</div>

## 対話モデルをテストする {#TestInteractionModel}

[対話モデルのビルド](#BuildInteractionModel)が完了すると、対話モデルをテストできます。次のように発話をテストできます。

<ol>
  <li>左側のサイドメニューの<strong>{{ book.DevConsole.cek_test }}</strong>をクリックします。<strong>{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.cek_test }}</strong>画面が表示されます。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Test_Menu.png" />
  <li><strong>{{ book.DevConsole.cek_builder_test_expression_title }}</strong>フィールドにテストする発話を入力し、<strong>{{ book.DevConsole.cek_builder_test_request_test }}</strong>ボタンをクリックします。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Test_Utterance_Example.png" />
</ol>

テストが完了すると、次のような結果を確認できます。結果から、下記の項目を確認します。

* **{{ book.DevConsole.cek_builder_test_service_response }}**項目から、[登録したCustom Extension](/DevConsole/Guides/Register_Custom_Extension.md)が正しく応答しているか確認します。
* **{{ book.DevConsole.cek_builder_test_intent_result }}**項目と**{{ book.DevConsole.cek_builder_test_slot_result }}**項目から、インテントとスロットが正しく認識されているか確認します。
* **{{ book.DevConsole.cek_builder_test_request_json }}**項目から、CEKがCustom Extensionに送る[リクエストメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtRequestMessage)に異常がないか確認します。JSONファイルの内容を編集してから**{{ book.DevConsole.cek_builder_test_test_again }}**ボタンをクリックすると、再度テストできます。
* **{{ book.DevConsole.cek_builder_test_response_json }}**項目から、登録したCustom Extensionが正しく[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)を返しているか確認します。

![](/DevConsole/Assets/Images/DevConsole-Test_Result.png)

<!-- Start of the shared content: TestOnClovaApp -->

## ClovaアプリでCustom Extensionをテストする {#TestOnClovaApp}

実際のクライアントであるClovaアプリで、Custom Extensionをテストできます。そのためには、Custom Extensionの基本情報を登録するページの**{{ book.DevConsole.cek_tester }}**フィールドに、開発者やCustom Extensionをテストする人の<strong>{{ book.ServiceEnv.OrientedService }}アカウント</strong>を入力する必要があります。アカウントを追加して**{{ book.DevConsole.cek_save }}**ボタンをクリックすると、そのアカウントで認証されたClovaアプリで開発中のCustom Extensionをテストできます。Clovaアプリでテストを中止するには、入力したアカウント情報を削除します。

![](/DevConsole/Assets/Images/DevConsole-Add_Tester_ID_For_Custom_Extension.png)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>テスターIDを登録してしばらく待つと、Custom Extensionをテストできます。もし1時間以上経ってもCustom Extensionをテストできない場合には、フォーラムまたはClova事務局までお問い合わせください。</p>
</div>

<!-- End of the shared content -->
