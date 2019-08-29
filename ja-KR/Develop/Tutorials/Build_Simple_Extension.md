# 簡単なExtensionを作成する
このチュートリアルでは、サイコロを振ってくれというユーザーのリクエストに対し、1つのサイコロを振る簡単なExtensionを作成します。

Custom Extensionをサービスするには、以下の2つの要素が必ず必要です。

{% include "/Develop/Guides/RequiredComponents/Interaction_Model.md" %}

{% include "/Develop/Guides/RequiredComponents/Extension_Server.md" %}

次のステップで上記の2つの要素を作成し、登録する方法を説明します。

Extensionを作成する基本的な流れは、以下のようになります。
* ステップ1Extensionサーバーを用意する（個別作業）
* ステップ2Extensionの基本情報を登録する（Clova Developer Centerで作業）
* ステップ3対話モデルを登録する（Clova Developer Centerで作業）
* ステップ4Extensionの動作をテストする（Clova Developer Centerと実際のデバイスで作業）

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>こうやって作成したExtensionを実際にサービスするには、<a href="/DevConsole/Guides/Deploy_Custom_Extension.md">Extensionを配布する</a>を参照してください。</p>
</div>

## ステップ1Extensionサーバーを用意する {#Step1}

サンプルサイコロExtensionが、登録したスロットをうまく処理するかをテストする必要があります。

[1番目のチュートリアル](/Develop/Tutorials/Build_Simple_Extension.md)のように、2つのテスト方法があります。1つは、Clova Developer Centerで対話モデルの動作を確認する方法です。もう1つは、テスターIDを登録して、Clovaデバイスで実際の動作を確認する方法です。
このチュートリアルでは、対話モデルの動作のみ確認します。

<a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>に接続し、次のようにサンプルサイコロExtensionがサイコロの数をうまく認識することを確認します。

<ol>
  <li><p>サンプルサイコロの<strong>{{ book.DevConsole.cek_interaction_model }}</strong>項目内<strong>{{ book.DevConsole.cek_edit}}</strong>ボタンをクリックします。</p></li>
  <li><p>画面左上の<strong>{{ book.DevConsole.cek_builder_menu_build }}</strong>ボタンをクリックして、対話モデルをビルドします。</p></li>
  <li><p>ビルドが終わると、左側のメニューリストで<strong>{{ book.DevConsole.cek_test }}</strong>メニューを選択します。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_test_expression_title }}</strong>に複数のサイコロを振るというフレーズを入力します。例えば「サイコロを2つ振ってみて」と入力します。</p></li>
  <li><p>Enterキーまたは<strong>{{ book.DevConsole.cek_builder_test_request_test }}</strong>ボタンをクリックします。</p></li>
  <li>
    <p><strong>{{ book.DevConsole.cek_builder_test_result_title }}</strong>の<strong>{{ book.DevConsole.cek_builder_test_intent_result }}</strong>項目に<code>ThrowDiceIntent</code>、<strong>{{ book.DevConsole.cek_builder_test_slot_result }}</strong>項目に<code>diceCount</code>が表示され、<strong>{{ book.DevConsole.cek_builder_test_slot_data}}</strong>に入力したサイコロの数が表示されることを確認します。</p>
  	<img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slot_Test.png" />
    <div class="note">
    	<p><strong>メモ</strong></p>
    	<p>外部からアクセスできるExtensionサーバーのURIを登録していない場合、<strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>は「{{ book.DevConsole.cek_builder_test_no_response }}」と表示されます。</p>
  	</div>
  </li>
  <li><p>「サイコロを10個振って」「4つのサイコロを振れ」などのフレーズで、ステップ4～6を繰り返します。</p></li>
</ol>

うまく認識されない場合、さまざまなサンプル発話を追加して、認識率を向上させることができます。

### Extensionサーバー開発のヒント {#Tip}

Clovaは、ユーザーの音声入力を解析した結果をExtensionサーバーに送信します。サーバーは、受け取ったリクエストに対して適切に応答するように実装する必要があります。

* **解析された結果が、開発者によって登録されたインテント（[カスタムインテント](/Design/Design_Custom_Extension.md#CustomIntent)）に該当する場合、**

	ClovaはExtensionの実行、登録されたインテントの実行、Extensionの終了など3つのタイプから、1つのリクエストメッセージを送信します。サーバーは受け取ったメッセージに応じて、Extensionの起動、指定されたインテントの処理、Extensionの終了を処理し、結果を返す必要があります。

* **解析された結果が、Clovaでデフォルトで提供されているインテント（[ビルトインインテント](/Design/Design_Custom_Extension.md#BuiltinIntent)）に該当する場合、**

	Clovaはそれに応じてヘルプの案内、肯定、否定、実行のキャンセルなどのリクエストメッセージを送信し、サーバーはそれに応じて一般的な応答を返す必要があります。

## ステップ2Extensionの基本情報を登録する {#Step2}

<a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>に接続して、Extensionの基本情報を登録します。
主な項目は次のとおりです。

<ul>
  <li>Extensionの情報
    <ul>
      <li><p><strong>{{ book.DevConsole.cek_id }}</strong>：Extension固有IDです。通常パッケージ名とExtension名の組み合わせで作成します。サンプルサイコロExtensionのIDは、「my.clova.extension.sampledice」を入力します。</p></li>
    	<li><p><strong>{{ book.DevConsole.cek_invocation_name }}</strong>：Extensionを実行する際に呼び出す名前です。Clovaアプリやスピーカー機器で正しく音声認識される言葉に設定します。サンプルサイコロExtensionの呼び出し名は、「サンプルサイコロ」です。</p></li>
    </ul>
  </li>
  <li>サーバー設定を行う
    <ul>
      <li>
        <p><strong>{{ book.DevConsole.cek_service_endpoint_url }}</strong>：Clovaと通信するExtensionのREST APIサーバーです。外部からアクセスできるURIにする必要があります。ステップ1でサンプルサイコロのソースコードを実行したサーバーのURIを入力します。</p>
    		<div class="note">
    			<p><strong>メモ</strong></p>
    			<p>テスト段階ではHTTPも使用できますが、正式なサービスのためにはHTTPSを使用する必要があります。Extensionサーバーは、HTTPで80ポート、HTTPSで443ポートに設定してください。</p>
    		</div>
      </li>
      <li><p><strong>{{ book.DevConsole.cek_account_linking }}</strong>：認可サーバー（OAuth 2.0）でサードパーティサービスの会員情報と連携する場合にのみ使用されます。サンプルサイコロExtensionの場合、<strong>{{ book.DevConsole.cek_no }}</strong>に設定します。</p></li>
    </ul>
  <li>配布情報、プライバシーポリシーおよびコンプライアンス情報<br />
    <p>Extensionの配布と審査に必要な情報です。このチュートリアルを進める際には、必ずしも入力する必要はありません。</p>
  </li>
</ul>

## ステップ3対話モデルを登録する {#Step3}

<a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>で対話モデルを登録します。

このチュートリアルでサンプルサイコロは、ユーザーから数の指定なしにサイコロを振るようにリクエストされると、デフォルトで1つのサイコロを振ります。ここでは、このように1つのサイコロを振るという指示を処理する、簡単な対話モデルを使用することにします。サイコロの数を収集しないため、スロットを持たないインテントを1つ登録することになります。

### 新規のカスタムインテントを作成する
ここでは、サイコロを振れというリクエストに対し、1つのサイコロを振る、簡単なインテントを作成します。

<ol>
  <li><p>サンプルサイコロの<strong>{{ book.DevConsole.cek_interaction_model }}</strong>項目の<strong>{{ book.DevConsole.cek_edit }}</strong>ボタンをクリックします。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_list_title_intent }}</strong>の右側にある<img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" />ボタンをクリックします。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_new_intent }}</strong>の下の入力ウィンドウに「ThrowDiceIntent」と名前を入力します。</p></li>
  <li>
    <p>Enterキーまたは入力ウィンドウの右側にある<strong>{{ book.DevConsole.cek_builder_new_intent_create }}</strong>ボタンをクリックします。</p>
  	<img src="/Develop/Assets/Images/CEK_Tutorial_NewIntent.png" />
  	<div class="note">
  	  <p><strong>メモ</strong></p>
  		<p>インテント名の大文字・小文字の区別に注意します。</p>
  	</div>
  </li>
</ol>

### サンプル発話のリストに文章を入力する
ここでは、ユーザーが発する文章のうち、どんなものをインテントとして処理するか指定します。サンプル発話の数は多ければ多いほど良いですが、このチュートリアルでは1つだけ入力します。

<ol>
  <li><p><strong>{{ book.DevConsole.cek_builder_intent_expression_title }}</strong>に「サイコロを振って」と入力します。</p></li>
  <li><p>Enterキーを押すか、または<img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" />ボタンをクリックします。</p></li>
  <li>
    <p>すべてのサンプル発話を入力して、<strong>{{ book.DevConsole.cek_save }}</strong>をクリックします。</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_SpeechExample.png" style="margin-top:10px; margin-bottom:10px;" />
  </li>
</ol>

### ビルドおよびテストする
対話モデルが入力された通りに動作するか確認するために、対話モデルをビルドしてテストします。

<ol>
  <li>
    <p><strong>Custom Extension</strong>画面の左上にある<strong>{{ book.DevConsole.cek_builder_menu_build }}</strong>ボタンをクリックします。</p>
  	<div class="note">
  	  <p><strong>メモ</strong></p>
  		<p>ビルドは数分かかります。ビルドが開始されると、ボタンが<strong>{{ book.DevConsole.cek_builder_menu_build_in_progress }}</strong>に変わり、ビルドが完了すると<strong>{{ book.DevConsole.cek_builder_menu_build }}</strong>に戻ります。</p>
  	</div>
  </li>
  <li><p>ビルドが完了すると、<strong>{{ book.DevConsole.cek_builder_menu_build }}</strong>ボタンの下にある<strong>{{ book.DevConsole.cek_test }}</strong>メニューをクリックします。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_test_expression_title }}</strong>にテストする文章を入力します。例えば、「サイコロを振って」と入力します。</p></li>
  <li><p>Enterキーまたは<strong>{{ book.DevConsole.cek_builder_test_request_test }}</strong>ボタンをクリックします。</p>
  <li>
    <p><strong>{{ book.DevConsole.cek_builder_test_result_title }}</strong>の<strong>{{ book.DevConsole.cek_builder_test_intent_result }}</strong>項目に「ThrowDiceIntent」と表示されるか確認します。</p>
  	<img src="/Develop/Assets/Images/CEK_Tutorial_Test.png" />
  	<div class="note">
    	<p><strong>メモ</strong></p>
    	<p>ステップ2で、外部からアクセスできるExtensionサーバーのURIを登録していない場合、<strong>{{ book.DevConsole.cek_builder_test_service_response }}</strong>は「{{ book.DevConsole.cek_builder_test_no_response }}」と表示されます。</p>
  	</div>
  </li>
</ol>

## ステップ4Extensionの実際の動作をテストする {#Step4}

対話モデルが正しく動作することを確認したら、審査をリクエストする前に、実際のデバイスでテストして、音声の認識と応答が想定した通りに動作するか確認する必要があります。

### テスターIDを登録する

Extensionを特定のアカウントでのみ実行できるように、テスターIDを登録します。

1. <a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>に接続します。
2. サンプルサイコロの**{{ book.DevConsole.cek_skill_info }}**項目の**{{ book.DevConsole.cek_edit }}**ボタンをクリックします。
3. 表示された画面の**{{ book.DevConsole.cek_tester }}**項目に、あなたの{{ book.ServiceEnv.OrientedService }}アカウントIDを入力します。
4. **{{ book.DevConsole.cek_save }}**ボタンをクリックします。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>テスターIDを登録してしばらく待つと、Extensionをテストできるようになります。もし1時間以上経ってもExtensionをテストできない場合には、フォーラムまたはClova事務局までお問い合わせください。</p>
</div>

<div class="note">
	<p><strong>メモ</strong></p>
  <p>実際のデバイスでテストするには、<strong>{{ book.DevConsole.cek_skill_info }}</strong>に外部からアクセスできる実際のExtensionサーバーのURLを必ず登録する必要があります。</p></li>
</div>

### Clovaアプリで実行する

ClovaアプリでサンプルサイコロExtensionを実行します。

1. テストするデバイスにClovaアプリをインストールします。
2. テスターIDとして入力した{{ book.ServiceEnv.OrientedService }}アカウントでログインします。
3. テスト用のExtensionの呼び出し名で、音声指示を出します。例えば、「ねぇClova、サンプルサイコロを起動して」と指示します。
4. Clovaアプリが「サイコロを1つ振ります」と応答することを確認します。

Extensionが実際のデバイスでも正しく動作したら、サービスの準備は完了です。Clova Developer Centerで審査をリクエストして、Extensionを配布することができます。
