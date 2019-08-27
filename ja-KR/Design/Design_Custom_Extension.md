# Custom Extensionをデザインする

新規のCustom Extensionを作成する際、すでに持っている技術やサービスを、どうすればユーザーが心地よく利用できるか考慮して設計する必要があります。このドキュメントは、ユーザーの役に立つサービスを提供するために、Custom Extensionを設計する際に守るべき事項についてのガイドラインを提供します。ちなみに、Clovaを利用するユーザーには、スキルという名称で機能を提供することになります。ユーザーにスキルを提供するには、開発者はCustom Extensionを実装する必要があります。

ウェブサービスの情報照会、ショッピングおよび宅配サービス、対話型のゲーム、放送またはリアルタイムのブリーフィング、IoTデバイスの制御、その他の音声を使用したアクティビティやサービスを提供するCustom Extensionを作成することができます。Custom Extensionを設計するとき、以下の内容に準ずる必要があります。ちなみに、ここで扱っている内容はCustom Extension設計の基本推奨事項で、簡単な例を挙げながら説明されています。お持ちのビジネス経験とサービスの特性によって、もっと多くの可能性を持つCustom Extensionに設計・実装することもできます。

* [目標を設定する](#SettingGoal)
* [ユーザーシナリオスクリプトを作成する](#MakeUseCaseScenarioScript)
* [スキル名/呼び出し名を定義する](#DefineInvocationName)
* [対話モデルを定義する](#DefineInteractionModel)
* [応答タイプを決める](#DecideSoundOutputType)
* [継続的なアップデート](#ContinuousUpdate)

## 目標を設定する {#SettingGoal}

Custom Extensionの設計は、最初にExtensionの目標を設定することから始まります。Custom Extensionの目標とは、具体的にユーザーに何をどのように届けるかを決めることです。Custom Extensionの目標を設定すると、ユーザーにどのような機能を提供して、その機能をどのように使用するか予測する根拠となります。Custom Extensionの目標は、次のような根本的で抽象的な目標が一例として挙げられます。

```
ユーザーにピザの宅配サービスを提供する。
```

このように設定したCustom Extensionの目標は、もっと具体的な目標に書き換えることができます。具体的な目標を定義する際、次の項目を参考する必要があります。

* 具体的な目標は、ユーザーの立場で作成します。
* 具体的な目標は、ユーザーがどのようにCustom Extensionを呼び出すかを含めて作成します。
* 具体的な目標を達成するための必要条件と、獲得する結果を組み合わせて作成することをお勧めします。必要条件には、次のようなものがあります。
  - 事前に必要な動作または状態
  - Custom Extensionが必要とする機能やリソース（GPS、カメラ、マイク）
  - 外部サービスまたはプラットフォームの情報（モバイルデバイスの連絡先、SNSアカウントの情報）
* 具体的な目標の集合が、達成しようとしているCustom Extensionの目標の範囲をすべてカバーしているか確認します。
* 1つの具体的な目標は、サービスで区別して処理する1つのユーザーアクションの単位と同じレベルに作成することをお勧めします。

以下は、ピザの宅配サービスを想定して作成した具体的な目標の例です。

| 目標ID | カテゴリ                | 目標                                                            |
|------------|--------------------|---------------------------------------------------------------|
| #1         | サービスの呼び出し           | ユーザーが「ピザボットを起動して」または「ピザボットで_[登録した実行コマンド]_」で発話を始めると、ピザの宅配サービスを使用できる。    |
| #2         | 利用の提案または推奨     | ピザの宅配サービスが開始されると、ユーザーはピザの注文に必要なアクションの案内を受けることができる。 |
| #3         | メニューの検索および選択      | ユーザーはピザのメニューを確認して選択できる。                                   |
| #4         | サービス提供者の確認および選択     | ユーザーはピザのサービス提供者を選択できる。                                 |
| #5         | 注文および支払い          | ユーザーはピザの種類、数量および配達先の住所を入力して、ピザを注文できる。 |
| #6         | 利用の提案または推奨     | ユーザーがピザのサービス提供者を選択すると、最近の注文履歴からピザの種類、配達先および支払い方法が提案される。 |
| #7         | 利用の提案または推奨     | ユーザーが他のメニューをリクエストすると、Custom Extensionから別のメニューを推奨される。 |
| #8         | 注文および支払い          | ユーザーはカメラを使用してクーポンを利用できる。                    |
| #9         | 配達状況の照会             | 注文が完了すると、ユーザーはピザの準備状況および配達状況を確認できる。  |
| #10        | サービス終了            | ユーザーが希望する作業を完了すると、サービスを終了できる。
| ...        | ...                 | ...                                                            |

<div class="note">
  <p><strong>メモ</strong></p>
  <p>こうして作成された具体的な目標は、<a href="#MakeUseCaseScenarioScript">ユーザーシナリオのスクリプトを作成</a>するか、<a href="#DefineInteractionModel">対話モデル</a>を定義する際の基盤情報となります。また、<a href="/DevConsole/Guides/Deploy_Custom_Extension.md#InputDeploymentInfo">Extensionを配布</a>する際にその情報を登録する必要があります。その情報に基づいて、Custom Extensionが正しく動作するか<a href="/DevConsole/Guides/Deploy_Custom_Extension.md#RequestExtensionSubmission">審査</a>を受けます。</p>
</div>

## ユーザーシナリオスクリプトを作成する {#MakeUseCaseScenarioScript}

ユーザーシナリオのスクリプトは、ユーザーとClovaの対話をあらかじめ想定したものです。具体的な目標に基づき、様々なユーザーシナリオで起こるユーザーとClovaの対話を予測することで、サービスの利便性、流れなどを点検することができます。[目標を設定する](#SettingGoal)で決めた具体的な目標に基づいて、予想されるユーザーシナリオのスクリプトを作成します。後ほど[対話モデル](#DefineInteractionModel)を登録する際に再使用できます。

ユーザーシナリオのスクリプトは、次の事項を考慮して作成することをお勧めします。

* 書き言葉ではなく、話し言葉で作成してください。
* Custom Extensionが情報を提供する際、情報や選択肢を与えすぎないでください。
* Custom Extensionの応答に、ユーザーに対して次の動作に関する案内や、サービスの使用を推奨する案内が含まれる必要があります。
* 繰り返しの表現は避けることをお勧めします。
* 常に突発的なリクエストや状況が発生する可能性があるということを念頭に置く必要があります。

以下は、ユーザーシナリオを、連続するユーザーリクエストタイプと一時的な指示タイプに分けてスクリプトを作成した例です。
* 連続するユーザーリクエストタイプのシナリオ

| 発話の主体           | サンプル発話                                                                   | 関連する目標     |
|-------------------|---------------------------------------------------------------------------|----------------|
| ユーザー     | ピザボットを起動して。                                                                                                    | #1             |
| Extension | こんにちは。ピザボットです。どういったご用件ですか？                                                                             | #2             |
| ユーザー      | ピザの種類は何があるの？                                                                                                    | #3             |
| Extension | コンビネーションピザ、ペパロニピザ、スーパー・シュプリームピザがあります。どれを注文しますか？                                                   | #2、#3         |
| ユーザー      | コンビネーションピザを注文して。                                                                                             | #3             |
| Extension | 登録されたピザ屋は、XXピザとYYピザがあります。どちらにしますか？                                                                  | #2、#4         |
| ユーザー      | XXピザにして。                                                                                                     | #4             |
| Extension  | XXピザのXX店から、コンビネーションピザ1枚、コーラ1.5リットル1本をAA洞111番地に配達します。合計金額は23,000ウォン、お支払い方法は配達員に現金払いです。よろしいですか？ | #2、#6         |
| ユーザー      | 他のメニューにして。                                                                                                   | #7             |
| Extension | スーパー・シュプリームピザ1枚、コーラ1.5リットル1本をAA洞111番地に配達します。合計金額は23,000ウォン、お支払い方法は配達員に現金払いです。よろしいですか？          | #2、#7         |
| ユーザー     | うん。                                                                                                             | #5             |
| Extension | 注文が完了しました。                                                                                                | #2             |
| ユーザー     | ピザボットを終了して。                                                                                                      | #10            |

* 一時的な指示タイプのシナリオ

| 発話の主体   | サンプル発話                                              | 関連する目標  |
|---------|------------------------------------------------------|-------------|
| ユーザー      | ピザボットで注文の状況を確認して。                               | #1、#9      |
| Extension | ただいま配達中です。もう少しお待ちください。                 | #9          |

## スキル名/呼び出し名を定義する {#DefineInvocationName}

新しくCustom Extensionを作成する際には、**スキル名**と**呼び出し名**を定義する必要があります。**スキル名**とは、スキルストアに表示されるCustom Extensionの表示名です。**呼び出し名**は、ユーザーがCustom Extensionを起動する際にClovaに話しかけるための名称です。

**スキル名**と**呼び出し名**は、必ずしも同じ表記である必要はありません。ただし、**スキル名**と**呼び出し名**の内容が大きく異なる場合や、コンテンツの内容と無関係な名称、またはユーザーを混乱させる可能性のあるものは避けてください。

**スキル名**と**呼び出し名**は、次の条件を満たしている必要があります。

<table>
  <thead>
    <tr>
      <th style="width:40%">要件</th><th style="width:60%%">説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>単語1語、または汎用的な言葉でないこと</td>
      <td>独自のブランドやサービス名を除き、単語1語の名前は許可されません。また、「スポーツニュース」などのように、一般的に使われる言葉は許可されません。開発者・開発会社、またはサービス・ブランド名などを組み合わせることを推奨します。（例：「太郎のスポーツニュース」）</td>
    </tr>
    <tr>
      <td>人名や地名、場所でないこと</td>
      <td>人名や地名、場所のみで構成されたスキル名/呼び出し名は許可されません。ただし、人名や地名が名前の一部に使われている場合、審査を経て許可されることもあります。
        <ul>
          <li>例：「世宗大王」「李舜臣」「ソウル」「江南区」</li>
          <li>例外：「世宗大王のハングル教室」「ソウルの人気スポット」</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Clovaの機能に影響があるフレーズを含まないこと</td>
      <td>Clovaの機能に影響があるフレーズを含めることはできません。
        <ul>
          <li>Clovaの呼び出し名：「Clova」「ねぇClova」「サリー」「ジェシカ」「しんちゃん」</li>
          <li>Clovaの標準スキルの呼び出し語：「今日の天気」「主なニュース」など、Clovaの標準スキルを実行し、呼び出すフレーズ</li>
          <li><strong>呼び出し名</strong>を実行・終了するフレーズ：「を起動して」「を終了して」</li>
          <li><strong>呼び出し名</strong>と組み合わせる助詞：「～に」「～で」</li>
        </ul>
        <div class="note">
          <p><strong>メモ</strong></p>
          <p><strong>呼び出し名</strong>は、助詞と組み合わせたときに、違和感のない名前である必要があります。</p>
        </div>
      </td>
    </tr>
    <tr>
      <td>他スキルと同一または類似する名称でないこと</td>
      <td>他のスキルで利用されているスキル名/呼び出し名は使用しないでください。</td>
    </tr>
    <tr>
      <td>誤解を招く表現が含まれないこと</td>
      <td>以下に該当する呼び出し名は、許可されません。
        <ul>
          <li>第三者または、{{ book.ServiceEnv.OrientedService }}および関連会社のサービスと誤解される可能性がある。（例：NAVER釣りの達人）</li>
          <li><strong>スキル名</strong>や<strong>呼び出し名</strong>と、コンテンツの内容がそれぞれ異なる、または誤解を招く可能性がある。（例：スキル名が「ネコちゃんの鳴き声」で、呼び出し名が「ワンちゃんの鳴き声」）</li>
          <li>コンテンツの内容が名前と異なる、または誤解を招く可能性がある。（例：スキル名が「ネコちゃんの鳴き声」で、ワンちゃんの鳴き声をコンテンツとして提供する）</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>利用規約に違反していないこと</td>
      <td><a href="{{ book.ServiceEnv.CEKTermsOfUseURI }}" target="_blank">Clova Extensions Kit利用規約</a>を遵守しなければなりません。第三者の権利を侵害したり、わいせつな表現を用いたりした呼び出し名であってはなりません。</td>
    </tr>
    <tr>
      <td>その他の留意事項</td>
      <td>以下の留意事項があります。
        <ul>
          <li>例外として、独自のブランド名や知的財産、名前や場所などの固有名詞などは使用可能です。</li>
          <li><strong>呼び出し名</strong>はユーザーが正確に発話しやすく、他の言葉と聞き間違えにくいものである必要があります。</li>
          <li><strong>スキル名</strong>に読み方がわかるように<strong>呼び出し名</strong>を括弧書きで記載するなど、ユーザーがわかるように補足することをお勧めします。</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

<div class="note">
<p><strong>メモ</strong></p>
<p>ガイドラインは変更される可能性があり、すでに許可されたスキル名/呼び出し名が使用できなくなることがあります。あらかじめご了承ください。判断が難しい場合には、<a href="/DevConsole/Guides/Deploy_Custom_Extension.md#RequestExtensionSubmission">審査をリクエストするとき</a>にコメントを記載いただきますようお願いいたします。</p>
</div>

## 対話モデルを定義する {#DefineInteractionModel}

音声から認識されたユーザーのリクエストは、まずサービスの提供に必要なフォーマット（JSON）に変換され、Custom Extensionに渡されます。例えば、ピザの宅配サービスを提供するCustom Extensionがあると仮定します。その場合、ユーザーから「ペパロニピザを2枚注文して」のようなリクエストが入力されます。Clovaで対話モデルとは、このようなユーザーのリクエストを、次のようにサービスの提供に必要な標準形式（JSON）に変換するルールを定義したものです。

![](/Design/Assets/Images/Extension_Design-Interaction_Model_Analysis_Diagram.png)

対話モデルをClova Developer Centerで定義するには、まず対話モデルとは何であるかを理解する必要があります。対話モデルをきちんと理解せずに定義してしまうと、パフォーマンスが低いか、ユーザーのリクエストを正しく処理できないExtensionになる可能性があります。ユーザーの実際の意図をうまく把握する対話モデルを作成するためには、対話モデルを作成する前に、まず以下の内容を理解して、設計に反映する必要があります。

* [インテント](#Intent)
* [スロット](#Slot)
* [サンプル発話](#UtteranceExample)

### インテント {#Intent}

インテントは、Custom Extensionが処理するユーザーのリクエストを区分したカテゴリです。主にユーザーの発話で使われた**動詞**型の要素によって区別されます。インテントには、カスタムインテントとビルトインインテントの2種類があります。

* [カスタムインテント](#CustomIntent)
* [ビルトインインテント](#BuiltinIntent)

#### カスタムインテント {#CustomIntent}

カスタムインテントはビルトインインテントとは違って、提供するサービスに特化したユーザーリクエストのカテゴリを定義します。次のような内容が定義されています。
* サービスが処理するユーザーリクエストのカテゴリ
* ユーザーリクエストのカテゴリごとに必要とする情報（[スロット](#Slot)）
* ユーザーリクエストのカテゴリごとに予想される様々な[サンプル発話](#UtteranceExample)

先ほど例として挙げたピザの宅配サービスの場合、次のようなリクエストのカテゴリを考えられます。

* メニュー確認のリクエスト
* 注文のリクエスト
* 配達状況確認のリクエスト

ピザの宅配サービス（Extension）の対話モデルを定義するということは、メニュー確認インテント、注文インテント、配達状況確認インテントなどのインテントのリストを宣言し、それぞれのインテントが必要とする情報（スロット）とサンプル発話を列挙するということです。最初に**対話モデルを定義する際、Custom Extensionが処理するリクエストのカテゴリを定義し、列挙する**必要があります。これはCustom Extensionを開発する際に、ビジネスロジックでプログラムの分岐を分ける基準にもなります。

![](/Design/Assets/Images/Extension_Design-Design_Interaction_Model.png)

**ユーザーリクエストのカテゴリを区分したら、次はカテゴリごとに名前を定義します。**これがインテント名です。ピザの宅配サービスの「注文インテント」のようなものは、あくまで抽象的な概念です。それをCustom Extensionが認識できる具体的な名前、つまり識別しやすい文字列として宣言する必要があります。例えば、「注文インテント」は、「OrderPizza」のような名前で宣言することができます。

次に、「OrderPizza」インテントがユーザーの発話から**取得する情報（[スロット](#Slot)）を定義**し、処理できる**様々な[サンプル発話](#UtteranceExample)を列挙**します。

#### ビルトインインテント {#BuiltinIntent}

ビルトインインテントは、Clovaプラットフォームが一部の共通したユーザーリクエストのカテゴリを決め、それを共有するために宣言した仕様です。頻繁に発生するインテントとして、次のようなリクエストがあらかじめ定義されています。

| ビルトインインテントの名前       | 意図               | ユーザーのサンプル発話                                      |
|---------------------------|-------------------|----------------------------------------------------------|
| `Clova.CancelIntent`                | 実行キャンセルのリクエスト        | キャンセル                                          |
| `Clova.GuideIntent`                 | ヘルプのリクエスト          | 使い方を教えて                           |
| `Clova.NextIntent`                  | 次のコンテンツをリクエストする      | 次の曲を再生して                                       |
| `Clova.NoIntent`                    | 否定の返事（いいえ、No） | いいえ                                     |
| `Clova.PauseIntent`                 | 再生を一時停止するようにリクエストする   | ちょっと止めて                                       |
| `Clova.PreviousIntent`              | 前のコンテンツをリクエストする      | 前の曲を再生して                                      |
| `Clova.RequestAlternativesIntent`   | 新しいコンテンツを再生するようにリクエストする | 他の曲をプレイして                    |
| `Clova.ResumeIntent`                | 再生を再開するようにリクエストする       | 再生を再開して                                  |
| `Clova.StopIntent`                  | 再生を停止するようにリクエストする       | 停止して                                                 |
| `Clova.YesIntent`                   | 肯定の返事（はい、Yes）   | はい                   |

### スロット {#Slot}

スロットは、ユーザーの発話から取得される情報です。ユーザーの発話で使われた**名詞**型の要素がスロットになります。[カスタムインテント](#Intent)を定義する際、そのインテントが必要とするスロットを一緒に定義する必要があります。ソフトウェアの開発に例えると、インテントは特定のカテゴリのユーザーリクエストを処理する関数またはハンドラーで、スロットはその関数やハンドラーに必要なパラメータに該当します。先ほど例として挙げた「ペパロニピザを2枚注文して」というユーザーの発話の場合、「OrderPizza」インテントを処理するには、「ペパロニピザ」のような、ピザの種類に関する情報と、「2枚」のような数量に関する情報が必要なことがわかります。インテントを定義する際、そのインテントがどんな情報（スロット）を必要とするかをあらかじめ把握しておく必要があります。

また、スロットを宣言する際には、そのスロットがどんなタイプの情報かを区分する必要があります。それがスロットタイプです。スロットタイプは、ビルトインスロットタイプとカスタムスロットタイプの2種類があります。

* [ビルトインスロットタイプ](#BuiltinSlotType)
* [カスタムスロットタイプ](#CustomSlotType)

#### ビルトインスロットタイプ {#BuiltinSlotType}

ビルトインスロットタイプは、Clovaであらかじめ定義されている情報タイプです。すべてのサービス（Extension）で共通で使用できる情報の表現が定義されています。ビルトインスロットタイプは、主に時間、場所、数量などのような情報を認識する必要がある場合に使用されます。上記の発話の場合、「2枚」に該当する情報を認識するためにビルトインスロットタイプを使用することができます。Clovaは、次のようなビルトインスロットタイプを提供しています。

| ビルトインスロットタイプ | 説明                                            |
| ----------------------|------------------------------------------------|
| `CLOVA.DATETIME`      | 日付や時刻表現を提供します。（例：「10分30秒」「午前9時」「1時間前」「12時」「正午」「2017年8月4日」「前月最終日」） |
| `CLOVA.DURATION`      | 期間の表現を提供します。（例：「1日」「1晩」「1か月」「来週」「週末」） |
| `CLOVA.NUMBER`        | 数字表現を提供します。助数詞を含みます。（例：「一度」「7人」「1つ」「30歳」「8くらい」「16マス」） |
| `CLOVA.RELATIVETIME`  | 相対的な時間表現を提供します。（例：「これから」「後で」「しばらくして」「ついさっき」「先ほど」） |
| `CLOVA.UNIT`          | 単位表現を提供します。（例：「113坪」「100MB」「25マイル」） |
| `CLOVA.ORDER`        | 順序表現を提供します。（例：「ネクスト」「先」「前」「最後」「次」「今度」「以前」「最終」） |
| `CLOVA.KO_ADDRESS_[行政区画の単位]` | 国内の行政区画の単位による地名を提供します。Clovaで提供される行政区画の単位は、Clova Developer Centerから確認できます。 |
| `CLOVA.WORLD_COUNTRY` | 世界の国名を提供します。（例：「ガーナ」「日本」「韓国」「フランス」） |
| `CLOVA.WORLD_CITY`   | 世界の都市名を提供します。（例：「ニューヨーク」「パリ」「ロンドン」） |
| `CLOVA.CURRENCY`      | 通貨表現を提供します。（例：「元」「円」「ドル」「ロシアの貨幣」「イギリスの通貨」） |
| `CLOVA.OFFICIALDATE ` | 休日および祝日、記念日を提供します。（例：「立春」「お正月」「釈迦（しゃか）の誕生日」「独立記念日」） |

#### カスタムスロットタイプ {#CustomSlotType}

カスタムスロットタイプは、提供するサービス（Extension）のドメインに特化した情報のタイプを定義したものです。カスタムスロットタイプは、主に固有名詞または一般名詞を指定して作成します。例えば、上記の発話で「OrderPizza」インテントで、ピザの種類に該当する情報（スロット）をユーザーの発話から把握する必要があります。また、ピザの種類を示す表現は、ピザに関するサービスでのみ使用される可能性が高いです。従って、「PIZZA_TYPE」のようなカスタムスロットタイプを定義し、そのカスタムスロットタイプに、ピザの宅配サービスで注文できる「ペパロニピザ」「コンビネーションピザ」「チーズピザ」などの項目が使用されると宣言することができます。

ただし、同じ意味を持つ言葉でも、実際の発話では様々な言い方で表現されることがあります。「バーベキューピザ」は「BBQピザ」のような同義語を持ち、「シュリンプ・ゴールドクラストピザ」のような長い名前の場合、「シュリンプ・ゴルクラ」のようによく使われる短縮表現が存在する可能性があります。従って、カスタムスロットタイプを定義する際には、概念的に区別される項目だけでなく、その項目ごとに代表語と同義語・類義語を定義する必要があります。そうすることで、ユーザーの発話を認識する際に、様々な言い方で表現される同義語や類義語を代表語に変換できます。また、Custom Extensionがインテントを処理する際に、同じ概念に該当する情報の表現のゆれを解消し、統一した値でExtensionに渡すことができます。

上記のようにスロットタイプを定義したら、次はそれぞれのインテントで使用するスロットの名前を定義し、そのスロットがどんなスロットタイプを持つか宣言する必要があります。例えば、「OrderPizza」インテントは、ピザの種類に関する情報のために「pizzaType」、ピザの数量に関する情報のために「pizzaAmount」というスロットを宣言し、スロットごとにあらかじめ定義した「PIZZA_TYPE」のカスタムスロットタイプと、すでに提供されている「CLOVA.NUMBER」のビルトインスロットタイプを指定することができます。

### サンプル発話 {#UtteranceExample}
インテントを定義する際、様々なのサンプル発話を列挙することができます。サンプル発話は、Clovaが同じ意図を持つ様々な言い方を認識するにあたって必要な基本データで、ユーザーの発話のうち、スロットに該当する情報を把握する際に利用されます。サンプル発話を適切に入力しておくと、ユーザーの意図を正しく認識する対話モデルを作成することができます。サンプル発話は、可能な限り次の事項に従って作成することを強くお勧めします。

* 同じ意図を表しているが、違う言い方で表現されるサンプル発話を様々なバリエーションで十分に入力してください。
* パターンが重複しないように、言い方には様々なバリエーションを持たせて発話サンプルを作成してください。
* サンプル発話の数は、次の基準に従ってください。
  * インテントにビルトインスロットタイプが使用されている、または、人が管理可能な量の辞書を持つカスタムスロットタイプの場合、そのスロットが含まれるサンプル発話を少なくとも30個作成してください。
  * インテントにアーティスト名、曲名、映画のタイトル、会社名など、サイズが大きな辞書を持つスロットタイプが使用されている場合、そのスロットが含まれる文章を少なくとも100個は作成してください。
  * 表現がシンプルなインテントであれば、10個程度のサンプル発話を入力してください。
* 上記の基準でサンプル発話を入力した後、新しい表現やうまく認識されない表現が見つかった場合には、サンプル発話を追加してください。
* スロットタイプの辞書に、スロットに該当するかどうかの判断が難しい値がある場合には、その値をサンプル発話として使用することで、スロットであることを明示してください。しかし、スロットタイプに曖昧な値が含まれないようにしてください。

サンプル発話をたくさん入力しながらも、パターンが重複しないようにするという推奨事項について、以下の図を参照してください。

![](/Design/Assets/Images/Extension_Design-Diagram_for_Utterance_Example.png)

例えば、ピザの宅配サービスで、注文に関するインテント（OrderPizza）に、以下のようなサンプル発話を作成したと仮定します。

```
ペパロニピザ1枚頼んで。
ペパロニピザ1枚注文してちょうだい。
ペパロニピザ1枚出前取って。
ペパロニピザ1枚宅配でお願い。
```

Clovaが上記のサンプル発話で学習した場合、「ペパロニ」や「1枚」という値がユーザーの発話に含まれると、その発話は`OrderPizza`インテントとして認識される可能性が高いです。例えば、「ペパロニピザ1枚いくらなの？」などのように、メニューの検索を予想した発話が、ピザの注文をリクエストした発話として処理される可能性があります。

そのようなことを防ぐために、サンプル発話は以下のように作成することを推奨します。

```
ペパロニピザ2枚頼んで。
BBQピザ1つ出前取ってくれる？
コンビネーション3つ宅配でお願い。
シュリンプ・ゴルクラピザ早くお願い、お腹空いてるの。
```

この例では発話が名詞（ピザの種類）、名詞（数量）、動詞（意図）のパターンで構成されていて、日常で使用される助詞、語尾、副詞、感嘆詞なども含まれています。発話のパターンをすべて洗い出すと、これらのパターンを再利用して、スロットの値を変えながらサンプル発話を推奨基準まで追加します。その際、次の事項に注意してください。
* サンプル発話に使用されたスロットの値を発話毎に変更して文章を追加してください。
* 助詞、語尾、副詞、感嘆詞などの使用にバリエーションを与えながら文章を追加します。
* **特定の値の組み合わせが何度も使用されないように注意します。**例えば、「ペパロニピザ2枚頼んで」と「ペパロニピザを1枚だけ頼んでください」の場合、語尾、助詞、数量は変わっていますが、「ペパロニ」と「頼む」という値の組み合わせは重複しています。

```
バーベキュー5つ早く注文して。
ペパロニ1枚食べるからね。
ベジタリアンピザお願い。
おいしいチーズピザを4つ注文してちょうだい。
```

「OrderPizza」インテントに関する発話をテキストで列挙し、スロットに該当する部分を次のように表示します。

{% raw %}

```
<pizzaType>ペパロニピザ</pizzaType><pizzaAmount>2枚</pizzaAmount>頼んで。
<pizzaType>BBQピザ</pizzaType><pizzaAmount>1つ</pizzaAmount>出前取ってくれる？
<pizzaType>コンビネーション</pizzaType><pizzaAmount>3つ</pizzaAmount>宅配でお願い。
<pizzaType>シュリンプ・ゴルクラピザ</pizzaType>早くお願い、お腹空いてるの。
<pizzaType>バーベキュー</pizzaType><pizzaAmount>5つ</pizzaAmount>早く注文して。
<pizzaType>ペパロニ</pizzaType><pizzaAmount>1枚</pizzaAmount>食べるからね。
<pizzaType>ベジタリアンピザ</pizzaType>お願い。
おいしい<pizzaType>チーズピザ</pizzaType>を<pizzaAmount>4つ</pizzaAmount>注文してちょうだい。
...
```
{% endraw %}

<div class="note">
  <p><strong>メモ</strong></p>
  <p><a href="/DevConsole/Guides/Test_Custom_Extension.md#TestInteractionModel">対話モデルのテスト</a>や、実際のユーザーのログを参照して調整することにより、完成度を高めることができます。対話モデルをテストする際には、サンプル発話の作成者ではなく、第三者がテストすることをお勧めします。そうすることで、新たな発話パターンを見つけることができます。</p>
</div>


[Clova Developer Center](/DevConsole/ClovaDevConsole_Overview.md)で[対話モデルを登録](/DevConsole/Guides/Register_Interaction_Model.md)すると、[登録されたCustom Extension](/DevConsole/Guides/Register_Custom_Extension.md)が次のようなJSONメッセージを受信します。

{% raw %}

```json
// カスタムインテント：ペパロニピザ2枚注文してちょうだい。
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
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b"
      }
    }
  },
  "request": {
    "type": "IntentRequest",
    "intent": {
      "name": "OrderPizza",
      "slots": {
        "pizzaAmount": {
          "name": "pizzaAmount",
          "value": "2"
        },
        "pizzaType": {
          "name": "pizzaType",
          "value": "ペパロニ"
        }
      }
    }
  }
}

// ビルトインインテント：キャンセルして
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
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b"
      }
    }
  },
  "request": {
    "type": "IntentRequest",
    "intent": {
      "name": "Clova.CancelIntent",
      "slots": {}
    }
  }
}
```

{% endraw %}

## 応答タイプを決める {#DecideSoundOutputType}

Custom Extensionは、ユーザーのリクエストを処理し、その結果をClovaを介してユーザーに伝える必要があります。Clovaは、基本的にユーザーとの対話に音を使用し、リクエストを受け付けるときはもちろん、ユーザーに返事をするときも音を使用します。Clovaは、音を使用した応答タイプとして、以下のものを提供します。Custom Extensionは、ユーザーシナリオに応じ、適切なタイプの応答を提供する必要があります。

* [音声出力タイプ](#OutputSpeech)
* [オーディオコンテンツの再生タイプ](#AudioPlayer)

### 音声出力タイプ {#OutputSpeech}

文字で表された情報を音声合成技術を用いて加工し、音声（TTS）で出力するタイプです。主に、ユーザーの質問やリクエストに対する返事として使用されます。応答は1つ以上の音声出力（TTS）で構成されます。また、短い効果音（.mp3）を一緒に提供することができます。

音声出力タイプで応答を作成するとき、以下のルールに従う必要があります。
* 1つの応答当たり、500文字または90秒以内に作成することを推奨します。応答が長くなると、ユーザーにとってわかりにくくなる可能性があります。
* 効果音は、比較的長さの短いオーディオコンテンツを使用します。
* 効果音は、応答が始まるときに必ず一度だけ再生されるように構成する必要があります。
  * 正しい例：効果音（.mp3） > 音声出力（TTS） > 音声出力（TTS） > ...
  * 間違った例1：音声出力（TTS） > 効果音（.mp3） > 音声出力（TTS） > ...
  * 間違った例2：効果音（.mp3） > 音声出力（TTS） > 効果音（.mp3） > 音声出力（TTS） > ...
* 1つの応答は、10個以下の音声出力で構成することを推奨します。

以下は、音声出力タイプのサンプルです。
```
//サンプル1：短い案内フレーズ
「首都名を当てるクイズです」

//サンプル2：詳しい説明と問題で構成された応答
1番目のTTS：「各国の首都名を当てるクイズを途中でやめるには「やめる」と言ってください。問題を再度聞きたいときには「もう一度」、他の問題に移るときには「他の問題」と言ってください」
2番目のTTS：「アメリカの首都はどこですか」

//サンプル3：効果音が含まれた応答
1番目の効果音（.mp3）：正解の効果音を再生する
2番目のTTS：「正解です」
```

<div class="note">
  <p><strong>メモ</strong></p>
  <p>音声出力タイプの応答に関する実装の説明については、<a href="/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse">Custom Extensionでレスポンスを返す</a>を参照してください。</p>
</div>


### オーディオコンテンツの再生タイプ {#AudioPlayer}

音楽のように長い間オーディオコンテンツを再生するタイプで、主に音楽を提供するスキルで使用されます。このタイプは、長時間オーディオコンテンツを再生するときだけでなく、一時停止、再開、停止するときにも使用されます。オーディオコンテンツの再生タイプで応答するには、Custom Extensionが以下のようにオーディオを再生する必要があります。

* Custom Extensionは、ユーザーのリクエストがあると、クライアントがオーディオコンテンツを再生できるようにレスポンスメッセージ（`AudioPlayer.Play`）を返す必要があります。
* Custom Extensionは、ユーザーのリクエストがあると、いつでもオーディオコンテンツを一時停止または停止できる必要があります。
* Custom Extensionは、一時停止または停止したコンテンツを再開または再生できる必要があります。
* Custom Extensionは、前や次に曲戻し/曲送りできる必要があります。

以下は、オーディオコンテンツ再生タイプの簡単なユーザーシナリオです。

> <p class="ldiag">「クラシックボックスを起動して」</p>
> <p class="rdiag">「どんなクラシック音楽を再生しますか？」（TTS）</p>
> <p class="ldiag">「ヴィヴァルディの四季を再生して」</p>
> <p class="rdiag">「はい。ヴィヴァルディの四季を再生します」（TTS）</p>
> <p class="rdiag">ヴィヴァルディの四季第1楽章を再生するように指示する（AudioPlayer.Play）</p>
> <p class="ldiag">「Clova、ちょっと止めて」</p>
> <p class="rdiag">再生を一時停止するように指示する（PlaybackController.Pause）</p>
> <p class="ldiag">「Clova、再生を再開して」</p>
> <p class="rdiag">再生を再開するように指示する（PlaybackController.Resume）</p>
> <p class="ldiag">「次」</p>
> <p class="rdiag">ヴィヴァルディの四季第2楽章を再生するように指示する（AudioPlayer.Play）</p>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>オーディオコンテンツの再生タイプで応答する方法については、<a href="/Develop/Guides/Provide_Audio_Content.md">オーディオコンテンツを提供する</a>を参照してください。</p>
</div>

## 継続的なアップデート {#ContinuousUpdate}

新規にCustom Extensionを開発する際には、ユーザーがどんなフレーズを発話するか予測してシナリオを作成し、Custom Extensionに適用します。Custom Extensionの公開後は、実際のユーザーの使い方が予想と必ず一致するとは限らず、またユーザーのすべての使用パターンを網羅できていない可能性もあります。ユーザーは想定していない方法でCustom Extensionを使用する可能性があるということです。UXを向上させるためには、Custom Extensionを公開してからも、Extensionの機能や対話の流れを継続的に改善する必要があります。

Custom Extensionを登録した後、Clovaプラットフォームで提供される統計データやユーザーの発話のログ（今後サービス予定）を分析し、随時Extensionをアップデートする必要があります。