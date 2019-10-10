# サンプルのExtension

Clovaでサービス提供中の一部のExtensionを紹介します。簡単なアクションを行うExtensionで、Extensionを開発する際に役立ちます。

* [マジックボール（Magic ball）](#MagicBall)
* [雨音（Rain sound）](#RainSound)
* [サイコロ遊び（Dice drawer）](#DiceDrawer)
* [コインヘルパー（Coin helper）](#CoinHelper)

## マジックボール（Magic ball） {#MagicBall}

ユーザーの質問に対し、あらかじめ定義した20種類の肯定または否定表現のうち1つを応答として返すExtensionです。

### 特徴
* ユーザーの発話とは関係なく応答を返すため、対話モデルが簡単です。
* クライアント/サーバープログラミングに例えると、「echo」レベルのサンプルといえます。
* Go言語で実装されています。

### GitHubリポジトリ
{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-magicball

## 雨音（Rain sound） {#RainSound}

雨音は、ユーザーのリクエストに対し、あらかじめキャプチャされた雨音のオーディオファイル（.mp3）をクライアントが再生するように応答するExtensionです。

### 特徴
* ユーザーは雨音を何回繰り返して聞くか選択できます。このExtensionの対話モデルには、繰り返し回数の値がスロットとして定義されています。
* クライアントがオーディオを再生できるように、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtRequestType)に案内および[`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play)ディレクティブを含めてCEKに送信します。
* Node.jsで実装されています。

### GitHubリポジトリ
{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-rainsound

## サイコロ遊び（Dice drawer） {#DiceDrawer}

サイコロ遊びは、ユーザーのリクエストに対し、仮想のサイコロを振って、出目の合計を返すExtensionです。

### 特徴
* ユーザーはサイコロを何個振るか選択できます。このExtensionの対話モデルには、サイコロの数に該当する値がスロットとして定義されています。
* 振るサイコロの数が1つか、または2つ以上かによって応答として返す表現が異なります。
* Node.jsで実装されています。

### GitHubリポジトリ
{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-dice

## コインヘルパー（Coin helper） {#CoinHelper}

コインヘルパーは、ユーザーのリクエストに対し、外部の仮想通貨の取引所から提供されるREST APIを呼び出し、相場情報を返すExtensionです。

### 特徴
* ユーザーは、照会する仮想通貨の種類と取引所を選択できます。このExtensionのinteraction modeには、取引所と仮想通貨の種類に関する値がスロットとして定義されています。
* 外部サービスのREST APIを利用して、他のサービスからデータを照会します。
* 他のサンプルより少し複雑な対話モデルを持っています。
* Go言語で実装されています。

### GitHubリポジトリ

{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-coinhelper
