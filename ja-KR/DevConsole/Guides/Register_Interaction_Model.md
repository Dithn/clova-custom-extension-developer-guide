# 対話モデルを登録する

CEKがCustom Extensionにユーザーのリクエストを送る際、ユーザーの発話をどう解析し、どんな形式で送るかを決めるために、[あらかじめ対話モデルを定義](/Design/Design_Custom_Extension.md#DefineInteractionModel)する必要があります。対話モデルは、[Custom Extension](/Develop/Guides/Build_Custom_Extension.md)に渡されるリクエストを標準化したスキームです。

対話モデルを登録するには、先にClova Developer Centerで[Custom Extensionを登録](/DevConsole/Guides/Register_Custom_Extension.md)する必要があります。次のように、CEKのメニューで対話モデルを登録するCustom Extensionの**{{ book.DevConsole.cek_edit }}**ボタンをクリックします。

![](/DevConsole/Assets/Images/DevConsole-Interaction_Model_Menu.png)

以下のような**{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.cek_builder_header_title_dashboard }}**画面が表示されます。

![](/DevConsole/Assets/Images/DevConsole-Interaction_Model_Dashboard.png)

Custom Extensionを設計する段階で[定義した対話モデル](/Design/Design_Custom_Extension.md#DefineInteractionModel)は、次の順で登録します。

1. [ビルトインスロットタイプを追加する](#AddBuiltinSlotType)
2. [カスタムスロットタイプを追加する](#AddCustomSlotType)
3. [ビルトインインテントを追加する](#AddBuiltinIntent)
4. [カスタムインテントを追加する](#AddCustomIntent)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>カスタムインテントを追加してから必要なスロットタイプを追加することもできますが、Clova Developer Centerの提供するUIの特性上、先にスロットタイプを追加してからインテントを追加することをお勧めします。</p>
</div>

## ビルトインスロットタイプを追加する {#AddBuiltinSlotType}

サービスを提供するCustom Extensionがどの[ビルトインスロットタイプ](/Design/Design_Custom_Extension.md#Slot)を使用するか決めたら、そのCustom Extensionの対話モデルにビルトインスロットタイプを追加する必要があります。例えば、ピザの宅配サービスを提供するCustom Extensionを作成する場合、ユーザーの発話に、ピザの数量に関する情報の表現が含まれることが予想されます。従って、この場合は数量に関するビルトインスロットタイプを使用する必要があり、次の手順でCustom Extensionに追加します。

<ol>
  <li><strong>{{ book.DevConsole.cek_builder_list_title_slottype }}</strong>パネルの右上、または左側のサイドメニューの<strong>{{ book.DevConsole.cek_builder_list_title_slottype }}</strong>の右上にある<img class="inlineImage" src="/DevConsole/Assets/Images/DevConsole-Plus_Button.png" />ボタンをクリックします。<strong>{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.SlotType }}</strong>画面が表示されます。</li>
  <li><strong>{{ book.DevConsole.cek_builder_new_slottype_builtin_title }}</strong>項目で必要なビルトインスロットタイプのチェックボックスをクリックします。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Built-in_Slot_Type.png" />
  <li>必要なビルトインスロットタイプにチェックを入れ、右上の<strong>{{ book.DevConsole.cek_save }}</strong>ボタンをクリックします。</li>
</ol>

すると、**{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.cek_builder_header_title_dashboard }}**画面の**{{ book.DevConsole.cek_builder_list_title_slottype }}**パネルに、次のようにビルトインスロットタイプが追加されたことを確認できます。

![](/DevConsole/Assets/Images/DevConsole-Added_Built-in_Slot_Type.png)

## カスタムスロットタイプを追加する {#AddCustomSlotType}

次に、Custom Extensionで使用する[カスタムスロットタイプ](/Design/Design_Custom_Extension.md#Slot)を定義します。[ビルトインスロットタイプを追加する](#AddBuiltinSlotType)と同様にピザの宅配サービスCustom Extensionを例にすると、発話中にピザの種類に該当する部分をカスタムスロットタイプとして定義できます。以下の表のような代表語と同義語を持つ、「PIZZA_TYPE」というカスタムスロットタイプを追加すると仮定します。

| 代表語           | 同義語                                        |
|----------------|----------------------------------------------|
| ペパロニ          | ペパロニピザ                                  |
| バーベキュー           | バーベキューピザ、BBQピザ                           |
| チーズ             | チーズピザ                                     |
| 野菜             | 野菜ピザ、ベジピザ、ベジタリアンピザ               |
| シュリンプゴールドクラスト | シュリンプゴールドクラストピザ、シュリンプゴルクラピザ、シュリンプゴルクラ |

次の順でカスタムスロットタイプを追加します。

<ol>
  <li><strong>{{ book.DevConsole.cek_builder_list_title_slottype }}</strong>パネルの右上、または左側のサイドメニューの<strong>{{ book.DevConsole.cek_builder_list_title_slottype }}</strong>の右上にある<img class="inlineImage" src="/DevConsole/Assets/Images/DevConsole-Plus_Button.png" />ボタンをクリックします。<strong>{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.SlotType }}</strong>画面が表示されます。</li>
  <li><strong>{{ book.DevConsole.cek_builder_new_intent }}</strong>の入力フィールドに追加するカスタムスロットタイプ名を入力し、<strong>{{ book.DevConsole.cek_create }}</strong>ボタンをクリックします。カスタムスロットタイプが生成されると、そのカスタムスロットタイプの詳細を確認できる画面が表示されます。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Slot_Type_1.png" />
  <li><img class="inlineImage" src="/DevConsole/Assets/Images/DevConsole-Plus_Button.png" />ボタンをクリックして、<strong>{{ book.DevConsole.cek_builder_slottype_dictionary_title }}</strong>に代表語を追加します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Slot_Type_2.png" />
  <li>追加した代表語に、同義語や類義語を追加します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Slot_Type_3.png" />
  <li>最後に、右上の<strong>{{ book.DevConsole.cek_save }}</strong>ボタンをクリックします。</li>
</ol>

右の<strong>ダッシュボード</strong>メニューで**{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.cek_builder_header_title_dashboard }}**に移動すると、カスタムスロットタイプが追加されたことを確認できます。

![](/DevConsole/Assets/Images/DevConsole-Added_Custom_Slot_Type.png)

定義するカスタムスロットタイプに大量の情報を入力する必要がある場合、TSV（タブ区切りの値、.tsv）形式のファイルをアップロードすることもできます。TSVファイルの各行の1番目の値が代表語となります。それに続くタブ区切りの値は、その代表語の同義語または類義語になります。次は、「PIZZA_TYPE」というカスタムスロットタイプの定義を、TSV形式で表現したものです。

```
ペパロニ          ペパロニピザ
バーベキュー      バーベキューピザ      BBQピザ
チーズ          チーズピザ
野菜        野菜ピザ        ベジピザ        ベジタリアンピザ
シュリンプゴールドクラスト    シュリンプゴールドクラストピザ    シュリンプゴルクラピザ      シュリンプゴルクラ
```

Clova Developer Centerは、以下のように**アップロード**ボタンと**ダウンロード**ボタンを提供します。**アップロード**ボタンをクリックすると、あらかじめTSVファイルで定義したカスタムスロットタイプをアップロードできます。**ダウンロード**ボタンをクリックすると、現在作成しているのカスタムスロットタイプをTSV形式でダウンロードできます。

![](/DevConsole/Assets/Images/DevConsole-Custom_Slot_Upload_and_Download_Button.png)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>すでにカスタムスロットが登録されている状態でTSVファイルをアップロードすると、既存のデータが削除され、TSVファイルのデータに置き換えられます。既存データを削除しないようにするには、事前に既存データをダウンロードし、追加分と合わせたTSVファイルを作成してアップロードしてください。</p>
</div>

## ビルトインインテントを追加する {#AddBuiltinIntent}

Clovaプラットフォームでは、共通したユーザーリクエストのカテゴリを決め、それを共有して利用するために[ビルトインインテント](/Design/Design_Custom_Extension.md#BuiltinIntent)が提供されています。例えば、頻繁に発生すると予想されるユーザーの肯定・否定、キャンセルなどのリクエストを、インテントとしてあらかじめ定義したものです。すべてのCustom Extensionは、Clovaが提供するすべてのビルトインインテントに対応する必要があります。デフォルトで登録されているビルトインインテントは、以下のとおりです。

![](/DevConsole/Assets/Images/DevConsole-Built-in_Intent_List.png)

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>今後Custom Extensionごとに必要なビルトインインテントのみ使用できるように変更される予定です。</p>
</div>

## カスタムインテントを追加する {#AddCustomIntent}
Custom Extensionが使用する[ビルトインスロットタイプ](#AddBuiltinSlotType)と[カスタムスロットタイプ](#AddCustomSlotType)を追加したら、次はカスタムインテントを追加します。続けて、ピザを注文するユーザーのリクエストを仮定します。次の順で「OrderPizza」という名前のインテントを追加します。

<ol>
  <li><strong>{{ book.DevConsole.cek_builder_list_title_intent }}</strong>パネルの右上または左側のサイドメニューの<strong>{{ book.DevConsole.cek_builder_list_title_intent }}</strong>の右上にある<img class="inlineImage" src="/DevConsole/Assets/Images/DevConsole-Plus_Button.png" />ボタンをクリックします。<strong>{{ book.DevConsole.cek_interaction_model }}：{{ book.DevConsole.NewIntent }}</strong>画面が表示されます。</li>
  <li><strong>{{ book.DevConsole.CreateCustomIntent }}</strong>の入力フィールドに、追加するカスタムインテント名を入力し、<strong>{{ book.DevConsole.cek_create }}</strong>ボタンをクリックします。カスタムインテントが生成されると、そのカスタムインテントの詳細を確認する画面が表示されます。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Intent_1.png" />
  <li><strong>{{ book.DevConsole.cek_builder_intent_slot_title }}</strong>の入力フィールドに、追加するスロットの名前を入力し、右の<img class="inlineImage" src="/DevConsole/Assets/Images/DevConsole-Plus_Button.png" />ボタンをクリックしてスロットを追加します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Intent_2.png" />
  <li>スロットを追加し、そのスロットのタイプを指定します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Intent_3.png" />
  <li>次に、<strong>{{ book.DevConsole.cek_builder_intent_expression_title }}</strong>にサンプル発話を入力し、右の<img class="inlineImage" src="/DevConsole/Assets/Images/DevConsole-Plus_Button.png" />ボタンをクリックしてそのサンプル発話を追加します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Intent_4.png" />
  <li>追加したサンプル発話で、スロットとして処理される部分をドラッグしてスロットを指定します。</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Add_Custom_Intent_5.png" />
  <li>ステップ5、6を繰り返して、インテントにサンプル発話を必要なだけ追加します。</li>
  <li>最後に、右上の<strong>{{ book.DevConsole.cek_save }}</strong>ボタンをクリックします。</li>
</ol>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>スロットタイプとスロット名は、集合の名前、または複数の値を代入できるような抽象的な名前である必要があります。</p>
</div>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>1つのカスタムインテントに、サンプル発話を2000個まで登録できます。ただし、登録したサンプル発話の数と認識の精度は、必ずしも一致するわけではありません。詳細については、<a href="/Design/Design_Custom_Extension.md#UtteranceExample"></a>を参照してください。</p>
</div>

カスタムスロットタイプと同じく、TSV（タブ区切りの値、.tsv）形式のファイルをアップロードして定義することもできます。TSVファイルは、インテントのスロットを定義する部分と、サンプル発話を列挙する部分の2つに分けられます。インテントのスロット定義がファイルの先頭にあり、`[INTENT SLOT]`ヘッダの次の行でスロットが列挙されます。タブで区切られた最初の列はインテントで使用されるスロットの名前で、2番目の列はスロットタイプです。

インテントのサンプル発話の列挙がそれに続きます。`[INTENT EXPRESSION]`ヘッダの次の行でサンプル発話が列挙されます。サンプル発話でスロットを区別するために、該当する表現をスロット名のタグで囲む必要があります。以下は、インテントを定義したTSVファイルの例です。

```
[INTENT SLOT]
pizzaType	PIZZA_TYPE
pizzaAmount	CLOVA.NUMBER

[INTENT EXPRESSION]
<pizzaType>ペパロニ</pizzaType><pizzaAmount>2枚</pizzaAmount>注文して。
<pizzaType>BBQピザ</pizzaType><pizzaAmount>2枚</pizzaAmount>出前取ってくれる？
<pizzaType>コンビネーションピザ</pizzaType><pizzaAmount>2つ</pizzaAmount>頼んで。
<pizzaType>シュリンプゴルクラ</pizzaType><pizzaAmount>1個</pizzaAmount>お願い。
...
```

Clova Developer Centerは、以下のように**アップロード**ボタンと**ダウンロード**ボタンを提供します。**アップロード**ボタンをクリックすると、あらかじめTSVファイルで定義したカスタムインテントをアップロードできます。**ダウンロード**ボタンをクリックすると、現在作成しているカスタムインテントをTSV形式でダウンロードできます。

![](/DevConsole/Assets/Images/DevConsole-Utterance_Example_Upload_and_Download_Button.png)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>すでにカスタムインテントが登録されている状態でTSVファイルをアップロードすると、既存のデータが削除され、TSVファイルのデータに置き換えられます。既存データを削除しないようにするには、事前に既存データをダウンロードし、追加分と合わせたTSVファイルを作成してアップロードしてください。</p>
</div>

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>1つの対話モデルでは、インテントに関わらず、スロットタイプに同じ名前を宣言することをお勧めします。例えば、「OrderPizza」インテントで、ピザの種類（「PIZZA_TYPE」）に関するスロット名を「pizzaType」にしたとすると、他のインテントで同じスロットタイプを宣言する際、同じく「pizzaType」にする必要があります。ただし、「東京から大阪まで時間がどれぐらいかかるか教えて」のように、「東京」と「大阪」が同じスロットタイプであっても、目的によって区別される必要のある場合、スロット名を区別して作成します。</p>
</div>

これまで、1つのインテントを対話モデルに追加する方法を説明しました。この方法を繰り返し、Custom Extensionに必要なインテントを追加すると対話モデルが完成されます。

![](/DevConsole/Assets/Images/DevConsole-Added_Interaction_Model.png)
