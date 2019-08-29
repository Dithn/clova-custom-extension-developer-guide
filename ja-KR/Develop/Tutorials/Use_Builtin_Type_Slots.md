# ユーザーから入力された情報を活用する
このチュートリアルでは、[簡単なExtensionを作成する](/Develop/Tutorials/Build_Simple_Extension.md)で作成したサンプルサイコロExtensionで、ユーザーのリクエストに応じて1つ以上のサイコロを振る方法について説明します。

ユーザーの音声には、Extensionが実行するアクションの他にも、そのアクションに必要な追加情報が含まれていることがあります。[チュートリアルの概要](/Develop/Tutorials/Introduction.md)で説明されているExtensionの使い方を確認してください。

{% include "/Develop/Tutorials/BasicInformation/DICE_Sample_Dialog.md" %}

2つ目の発話で、ユーザーは「サイコロを**2つ**振って」とリクエストしています。ここでは、「2つ」が「サイコロを振る」というアクションに必要な追加情報です。

対話モデルでは、そのような追加情報を[スロット](/Design/Design_Custom_Extension.md#Slot)と呼びます。Extensionが追加情報を使用するには、対話モデルを定義するとき、想定される追加の入力値をあらかじめ把握し、適切なスロットを登録する必要があります。Clovaは登録されたスロットに基づいて、ユーザーのリクエストに含まれた追加情報を認識します。

追加情報は、以下のステップを経て、処理できるようになります。
* ステップ1対話モデルにスロットを登録する（Clova Developer Centerで作業）
* ステップ2スロットの処理を実装する（Extensionサーバーで作業）
* ステップ3スロットの動作をテストする（Clova Developer Centerで作業）

## ステップ1対話モデルにスロットを登録する {#Step1}

ユーザーがサイコロの数を指定できるように、対話モデルを修正する必要があります。

最初に、スロットが受け入れる情報のタイプを決め、対話モデルにスロットタイプを宣言し、そのタイプを使用するスロットをインテントに登録した後、追加情報が含まれたサンプル発話を入力して、Clovaがスロットを認識できるようにします。

### 使用するスロットタイプを宣言する
スロットが受け入れる情報のカテゴリに応じて、スロットタイプを決めます。例えば、サイコロの数は数字で表されます。

Clovaには、すべてのExtensionで汎用で使用できるように、一般的に使われる情報のカテゴリがあらかじめ定義されています。それを[ビルトインスロットタイプ](/Design/Design_Custom_Extension.md#BuiltinSlotType)といいます。数字の種類は`CLOVA.NUMBER`というビルトインスロットタイプで定義されているので、サイコロの数を処理するための別のタイプを作成する必要はありません。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>ビルトインスロットタイプとして定義されていない、Extension独自の情報のカテゴリは、<a href="/Design/Design_Custom_Extension.md#CustomSlotType">カスタムスロットタイプ</a>を定義して処理することができます。</p>
</div>

<a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>に接続し、次のようにサンプルサイコロExtensionで使用するスロットタイプを宣言します。

<ol>
  <li><p>サンプルサイコロの<strong>{{ book.DevConsole.cek_interaction_model }}</strong>項目の<strong>{{ book.DevConsole.cek_edit }}</strong>ボタンをクリックします。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_list_title_slottype }}</strong>の右側にある<img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" />ボタンをクリックします。</p></li>
  <li>
    <p><strong>{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}</strong>の下のテーブルで、<code>CLOVA.NUMBER</code>のチェックボックスを選択します。</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Register_Slot_Type.png" />
  </li>
  <li><p><strong>{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}</strong>の右側にある<strong>{{ book.DevConsole.cek_save }}</strong>ボタンをクリックします。</p></li>
</ol>

### インテントにスロットを登録する
振るサイコロの数は、サイコロを振るときに必要とされる追加情報です。サイコロを振るアクションは、1番目のチュートリアルで`ThrowDiceIntent`というインテントとして登録しました。次に、そのインテントにサイコロの数に対応するスロットを登録する必要があります。
前にスロットタイプを宣言した画面で、次のようにスロットを登録します。

<ol>
  <li><p><strong>{{ book.DevConsole.cek_builder_list_title_intent }}</strong>の下にあるカスタムインテントの<code>ThrowDiceIntent</code>を選択します。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_intent_slot_title }}</strong>の下の入力スペースに「diceCount」と入力します。</p></li>
  <li><p>Enterキーや右側の<img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" />ボタンをクリックします。</p></li>
  <li><p> 登録した「diceCount」の右の<strong>{{ book.DevConsole.cek_builder_utterance_select_slot }}</strong>コンボボックスをクリックします。</p></li>
  <li>
    <p>表示されるリストから、先ほど登録した<strong>{{ book.DevConsole.cek_builder_select_slottype_builtin }}</strong>の<code>CLOVA.NUMBER</code>を選択します。</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Add_Slot.png" />
  </li>
  <li><p>画面の右上にある<strong>{{ book.DevConsole.cek_save }}</strong>ボタンをクリックします。</p></li>
</ol>

### サンプル発話を入力する
1番目のチュートリアルでは、サンプル発話として、サイコロを振れという単純なフレーズを入力しましたが、ここではスロットを使用して、サイコロの数を指定する新しいフレーズを入力してみます。

先ほどスロットを登録した画面で、次のようにサンプル発話を入力します。

<ol>
  <li>
    <p><strong>{{ book.DevConsole.cek_builder_intent_expression_title }}</strong>の下の入力スペースに「サイコロを2つ振って」と入力します。</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Sample_Utterance.png" />
  </li>
  <li><p>Enterキーを押すか、または<img class="inlineImage" src="/Develop/Assets/Images/DevConsole_Plus_Button.png" />ボタンをクリックします。</p></li>
  <li><p>登録したフレーズの「2つ」という単語をマウスでドラッグして選択します。</p></li>
  <li>
    <p><strong>{{ book.DevConsole.cek_builder_slot_layer_select_slot }}</strong>の下にある「diceCount」を選択します。</p>
    <img src="/Develop/Assets/Images/CEK_Tutorial_Builtin_Type_Slots_Set_Slot.png" />
  </li>
  <li><p>「1つ振って」「5つのサイコロを振って」というフレーズでステップ1～4を繰り返し行います。</p></li>
</ol>

## ステップ2スロットの処理を実装する {#Step2}

サンプルサイコロExtensionがスロットを処理できるように、コードを変更する必要があります。
ここでは、あらかじめ作成されたソースコードを参考にして、どの部分が変更されたかを確認するために、[1番目のチュートリアル](/Develop/Tutorials/Build_Simple_Extension.md)のリポジトリを再び使用します。
次のようにローカルリポジトリにアクセスして、`tutorial3`ブランチに切り替えます。

```bash
cd /path/to/your_sample_dice_directory
git fetch
git checkout tutorial3
```

サンプルサイコロExtensionは、`clova/index.js`ファイルの`intentRequest()`でスロットを処理します。

```javascript
intentRequest(cekResponse) {
  const intent = this.request.intent.name
  const slots = this.request.intent.slots

  switch (intent) {
    case "ThrowDiceIntent":
      let diceCount = 1
      if (!!slots) {
        const diceCountSlot = slots.diceCount
        if (slots.length != 0 && diceCountSlot) {
          diceCount = parseInt(diceCountSlot.value)
        }

        if (isNaN(diceCount)) {
          diceCount = 1
        }
      }
      ...
  }
  ...
}
```

上記のコードでみられるように、Clovaからのリクエストからスロットの情報を抽出（`this.request.intent.slots`）し、先ほど対話モデルに登録した「diceCount」というスロット（`slots.diceCount`）がある場合、その値を整数形式で読み取ります。そうやって読み取った値が振るサイコロの数になります。スロットがなかったり、整数形式でない場合、デフォルト値の1として判断されます。

変更されたコードをExtensionサーバーで実行します。

## ステップ3スロットの動作をテストする {#Step3}

サンプルサイコロExtensionが、登録したスロットをうまく処理するかをテストする必要があります。

[1番目のチュートリアル](/Develop/Tutorials/Build_Simple_Extension.md)のように、2つのテスト方法があります。1つは、Clova Developer Centerで対話モデルの動作を確認する方法です。もう1つは、テスターIDを登録して、Clovaデバイスで実際の動作を確認する方法です。
このチュートリアルでは、対話モデルの動作のみ確認します。

<a href="{{ book.ServiceEnv.DeveloperConsoleURI }}/cek/#/list" target="_blank">Clova Developer Center</a>に接続し、次のようにサンプルサイコロExtensionがサイコロの数をうまく認識することを確認します。

<ol>
  <li><p>サンプルサイコロの<strong>{{ book.DevConsole.cek_interaction_model }}</strong>項目の<strong>{{ book.DevConsole.cek_edit}}</strong>ボタンをクリックします。</p></li>
  <li><p>画面左上の<strong>{{ book.DevConsole.cek_builder_menu_build }}</strong>ボタンをクリックして、対話モデルをビルドします。</p></li>
  <li><p>ビルドが終わったら、左側のメニューリストで<strong>{{ book.DevConsole.cek_test }}</strong>メニューを選択します。</p></li>
  <li><p><strong>{{ book.DevConsole.cek_builder_test_expression_title }}</strong>に、複数のサイコロを振れというフレーズを入力します。例えば、「サイコロを2つ振ってみて」と入力します。</p></li>
  <li><p>Enterキーを押すか、または<strong>{{ book.DevConsole.cek_builder_test_request_test }}</strong>ボタンをクリックします。</p></li>
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

これで、サンプルサイコロExtensionは、1つ以上のサイコロを振ることができるようになりました。
同じやり方で、サイコロを1度以上振ったり、数字ではない目を持つサイコロを振ることもできます。
