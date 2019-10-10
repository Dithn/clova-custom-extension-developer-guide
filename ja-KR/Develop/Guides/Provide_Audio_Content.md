# オーディオコンテンツを提供する

{% if book.L10N.TargetCountryCode == "KR" %}
Custom Extensionで、ユーザーに音楽やポッドキャストなどのオーディオコンテンツを提供することができます。そのためには、[Custom Extensionのメッセージ](/Develop/References/Custom_Extension_Message.md)の[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージと[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)の仕様のうち、クライアント（ユーザーのデバイスまたはClovaアプリ）のためのメッセージ形式であるオーディオコンテンツ再生関連の[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})を使用する必要があります。ユーザーにオーディオコンテンツを提供するには、次の内容をExtensionに実装する必要があります。以下は、その詳細についての説明です。**特に、必須の実装項目は必ず実装してください。**

* 必須
  * [オーディオコンテンツの再生を指示する](#DirectClientToPlayAudio)
  * [オーディオコンテンツの再生をコントロールする](#ControlAudioPlayback)
  * [オーディオコンテンツのメタデータを提供する](#ProvidingMetaDataForDisplay)

* 任意
  * [再生状態の変更および進行状況を収集する](#CollectPlaybackStatusAndProgress)
  * [セキュリティのためにオーディオコンテンツのURIを更新する](#UpdateAudioURIForSecurity)
  * [再生コントロールの動作方法を変更する](#CustomizePlaybackControl)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>オーディオコンテンツを再生するCustom Extensionを作成するには、<a href="/DevConsole/Guides/Register_Custom_Extension.md">Clova Developer CenterにExtensionを登録する</a>とき、<a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">基本情報</a>の{{ book.DevConsole.cek_audioplayer }}項目で<strong>はい</strong>を選択する必要があります。</p>
</div>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Custom Extensionで音声（TTS）やオーディオコンテンツをオーディオファイル（.mp3）で提供する場合、HTTPSで提供することを推奨します。Clovaプラットフォームでは、現在、HTTPでのオーディオの提供もサポートされていますが、予告なしにHTTPS URIを持つオーディオを提供するように変更する可能性があります。</p>
</div>
{% elif book.L10N.TargetCountryCode == "JP" %}
Custom Extensionで、ユーザーに音楽やポッドキャストなどのオーディオコンテンツを提供することができます。そのためには、[Custom Extensionのメッセージ](/Develop/References/Custom_Extension_Message.md)の[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージと[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)の仕様のうち、クライアント（ユーザーのデバイスまたはClovaアプリ）のためのメッセージ形式である[オーディオコンテンツ再生関連のCIC API](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)を使用する必要があります。ユーザーにオーディオコンテンツを提供するには、次の内容をExtensionに実装する必要があります。以下は、その詳細についての説明です。  **特に、必須の実装項目は必ず実装してください。**

* 必須
  * [オーディオコンテンツの再生を指示する](#DirectClientToPlayAudio)
  * [オーディオコンテンツの再生をコントロールする](#ControlAudioPlayback)
  * [オーディオコンテンツのメタデータを提供する](#ProvidingMetaDataForDisplay)

* 任意
  * [再生状態の変更および進行状況を収集する](#CollectPlaybackStatusAndProgress)
  * [セキュリティのためにオーディオコンテンツのURIを更新する](#UpdateAudioURIForSecurity)
  * [再生コントロールの動作方法を変更する](#CustomizePlaybackControl)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>オーディオコンテンツを再生するCustom Extensionを作成するには、<a href="/DevConsole/Guides/Register_Custom_Extension.md">Clova Developer CenterにExtensionを登録する</a>とき、<a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">基本情報</a>の{{ book.DevConsole.cek_audioplayer }}項目で<strong>はい</strong>を選択する必要があります。</p>
</div>
{% endif %}

## オーディオコンテンツの再生を指示する {#DirectClientToPlayAudio}

{% if book.L10N.TargetCountryCode == "KR" %}
ユーザーから、音楽、または音楽のような形でオーディオコンテンツの再生をリクエストされたとき、そのオーディオコンテンツをユーザーのクライアントに送信する必要があります。ユーザーからのオーディオコンテンツ再生のリクエストが[`IntentRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtIntentRequest)タイプのリクエストでCustom Extensionに渡され、Custom Extensionはその`IntentRequest`タイプのリクエストメッセージに対する[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)を返す必要があります。そのとき、このメッセージに、クライアントがオーディオコンテンツを再生するように指示する[`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play)ディレクティブ（クライアントを制御するためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）が含まれる必要があります。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>提供するオーディオコンテンツは、<a href="/Design/Supported_Audio_Format.md">プラットフォームでサポートされる音声ファイルフォーマット</a>である必要があります。</p>
</div>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>再生指示に関連する内容は、オーディオコンテンツを提供するCustom Extensionの主要な機能で、必須の実装項目です。</p>
</div>

[`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play)ディレクティブの仕様を確認し、クライアントに送信するオーディオの情報を、次のようにCustom Extensionのレスポンスメッセージに含める必要があります。お持ちのオーディオの情報や特徴によって、作成するフィールドとオーディオの情報が異なることがあります。
{% elif book.L10N.TargetCountryCode == "JP" %}
ユーザーから、音楽、または音楽のような形でオーディオコンテンツの再生をリクエストされたとき、そのオーディオコンテンツをユーザーのクライアントに送信する必要があります。ユーザーからのオーディオコンテンツ再生のリクエストが[`IntentRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtIntentRequest)タイプのリクエストでCustom Extensionに渡され、Custom Extensionはその`IntentRequest`タイプのリクエストメッセージに対する[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)を返す必要があります。そのとき、このメッセージに、クライアントがオーディオコンテンツを再生するように指示する[`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play)ディレクティブ（クライアントを制御するためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）が含まれる必要があります。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>提供するオーディオコンテンツは、<a href="/Design/Design_Guideline_For_Client_Hardware.md#SupportedAudioFormat">プラットフォームでサポートされる音声ファイルフォーマット</a>である必要があります。</p>
</div>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>再生の指示に関連する内容は、オーディオコンテンツを提供するCustom Extensionの主要な機能で、必須の実装項目です。</p>
</div>

[`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play)ディレクティブの仕様を確認し、クライアントに送信するオーディオの情報を、次のようにCustom Extensionのレスポンスメッセージに含める必要があります。お持ちのオーディオの情報や特徴によって、作成するフィールドとオーディオの情報が異なることがあります。
{% endif %}

```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "card": {},
    "directives": [
      {
        "header": {
          "namespace": "AudioPlayer",
          "name": "Play"
        },
        "payload": {
          "audioItem": {
            "audioItemId": "90b77646-93ab-444f-acd9-60f9f278ca38",
            "episodeId": 22346122,
            "stream": {
              "beginAtInMilliseconds": 419704,
              "progressReport": {
                "progressReportDelayInMilliseconds": null,
                "progressReportIntervalInMilliseconds": 60000,
                "progressReportPositionInMilliseconds": null
              },
              "token": "eyJ1cmwiOiJodHRwczovL2FwaS1leC5wb2RiYmFuZy5jb20vY2xvdmEvZmlsZS8xMjU0OC8yMjYxODcwMSIsInBsYXlUeXBlIjoiTk9ORSIsInBvZGNhc3RJZCI6MTI1NDgsImVwaXNvZGVJZCI6MjI2MTg3MDF9",
              "url": "https://streaming.example.com/clova/file/12548/22618701",
              "urlPlayable": true
            },
            "type": "podcast"
          },
          "source": {
            "name": "Potbbang",
            "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
          },
          "playBehavior": "REPLACE_ALL"
        }
      }
    ],
    "outputSpeech": {},
    "shouldEndSession": true
  }
}
```

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p><a href="/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse">レスポンスメッセージで返す</a>とき、<code>response.outputSpeech</code>フィールドを指定することもできます。例えば、ユーザーに対して、「リクエストしたオーディオコンテンツを再生します」という音声出力（TTS）を先に再生してから、オーディオコンテンツの再生を開始することができます。</p>
</div>

{% if book.L10N.TargetCountryCode == "KR" %}
<div class="note">
  <p><strong>メモ</strong></p>
  <p><a href="{{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }}">CIC API</a>ディレクティブ</p>を作成するとき、「Optional」項目が<strong>空白</strong>になっているフィールドは必須なので、必ず作成する必要があります。</p>
</div>
{% elif book.L10N.TargetCountryCode == "JP" %}
<div class="note">
  <p><strong>メモ</strong></p>
  <p><a href="/Develop/References/CEK_API.md#CICAPIforAudioPlayback">CIC API</a>ディレクティブ</p>を作成するとき、「Optional」項目が<strong>空白</strong>になっているフィールドは必須なので、必ず作成する必要があります。</p>
</div>
{% endif %}

## オーディオコンテンツの再生を制御する {#ControlAudioPlayback}
ユーザーのクライアントがオーディオを再生しているときに、ユーザーが「前」「次」などのように再生制御に関連するフレーズを発した場合、ユーザーのリクエストが`IntentRequest`タイプのリクエストメッセージでCustom Extensionに送信されることがあります。現在、CEKは再生制御に関連するユーザーのインテントを、以下のような[ビルトインインテント](/Design/Design_Custom_Extension.md#BuiltinIntent)としてCustom Extensionに渡すようになっています。

* `Clova.NextIntent`
* `Clova.PauseIntent`
* `Clova.PreviousIntent`
* `Clova.RequestAlternativesIntent`
* `Clova.ResumeIntent`
* `Clova.StopIntent`

<div class="note">
  <p><strong>メモ</strong></p>
  <p>再生のコントロールに関連する内容は、必須の実装項目です。特に、<code>Clova.PauseIntent</code>と<code>Clova.StopIntent</code>ビルトインインテントに対応するアクションが実装されていないと、ユーザーにとって、サービスを「使いにくい」と感じる原因となります。</p>
</div>

{% if book.L10N.TargetCountryCode == "KR" %}
ユーザーが「ちょっと止めて」「再生を再開して」「停止して」などのように発話した場合、Custom Extensionは再生の一時停止、再生再開、再生停止のリクエストに対応する必要があります。その際、クライアントはそれぞれのリクエストに対し、`Clova.PauseIntent`、`Clova.ResumeIntent`、`Clova.StopIntent`ビルトインインテントを`IntentRequest`タイプのリクエストメッセージで受け取ります。Custom Extensionは、これに対し、それぞれ以下のディレクティブ（クライアントを制御するためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）を[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)でCEKに送信する必要があります。

* [`PlaybackController.Pause`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Pause)ディレクティブ：クライアントに、再生中のオーディオストリームを一時停止するように指示する
* [`PlaybackController.Resume`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Resume)ディレクティブ：クライアントに、オーディオストリームの再生を再開するように指示する
* [`PlaybackController.Stop`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Stop)ディレクティブ：クライアントに、オーディオストリームの再生を停止するように指示する
{% elif book.L10N.TargetCountryCode == "JP" %}
ユーザーが「ちょっと止めて」「再生を再開して」「停止して」などのように発話した場合、Custom Extensionは再生の一時停止、再生再開、再生停止のリクエストに対応する必要があります。その際、クライアントはそれぞれのリクエストに対し、`Clova.PauseIntent`、`Clova.ResumeIntent`、`Clova.StopIntent`ビルトインインテントを`IntentRequest`タイプのリクエストメッセージで受け取ります。Custom Extensionは、これに対し、それぞれ以下のディレクティブ（クライアントを制御するためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）を[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)でCEKに送信する必要があります。

* [`PlaybackController.Pause`](/Develop/References/CEK_API.md#Pause)ディレクティブ：クライアントに、再生中のオーディオストリームを一時停止するように指示する
* [`PlaybackController.Resume`](/Develop/References/CEK_API.md#Resume)ディレクティブ：クライアントに、オーディオストリームの再生を再開するように指示する
* [`PlaybackController.Stop`](/Develop/References/CEK_API.md#Stop)ディレクティブ：クライアントに、オーディオストリームの再生を停止するように指示する
{% endif %}

以下は、`PlaybackController.Pause`ディレクティブをCustom Extensionのレスポンスメッセージに含めたサンプルです。
```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "card": {},
    "directives": [
      {
        "header": {
          "namespace": "PlaybackController",
          "name": "Pause"
        },
        "payload": {}
      }
    ],
    "outputSpeech": {},
    "shouldEndSession": true
  }
}
```

ユーザーが「前」あるいは「次」に該当する発話をして、`Clova.NextIntent`または`Clova.PreviousIntent`ビルトインインテントを`IntentRequest`タイプのリクエストメッセージで受信すると、現在のプレイリストの前、あるいは次の曲に該当する[オーディオコンテンツを再生するように指示する（`AudioPlayer.Play`）](#DirectClientToPlayAudio)[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)を作成します。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>前または次に該当するオーディオコンテンツがなかったり、有効ではない場合、「再生する前または次の曲がありません」などの音声出力を<a href="/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse">レスポンスメッセージで返す</a>必要があります。</p>
</div>

ユーザーが「他の曲を再生して」（`Clova.RequestAlternativesIntent`）に該当する発話をした場合には、状況を判断して応答します。様ざまな種類の応答が可能です。以下は、`Clova.RequestAlternativesIntent`ビルトインインテントに該当する例です。
* まったく新しいプレイリストの[オーディオコンテンツを再生するように指示する（`AudioPlayer.Play`）](#DirectClientToPlayAudio)[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)を作成する
* 似たような曲を再生するように指示するレスポンスメッセージを作成する
* 次の曲を再生するように指示するレスポンスメッセージを作成する
* ユーザーの意図を具体的に確認するために[マルチターン対話を行う](/Develop/Guides/Do_Multiturn_Dialog.md)
* これ以上提供できるコンテンツがなかったり、ユーザーのリクエストを処理できない場合、[音声案内（TTS）を提供する](/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse)


## オーディオコンテンツのメタデータを提供する {#ProvidingMetaDataForDisplay}

{% if book.L10N.TargetCountryCode == "KR" %}
ユーザーのクライアントに[オーディオコンテンツの再生を指示する](#DirectClientToPlayAudio)[`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play)ディレクティブ（クライアントを制御するためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）には、タイトル、アルバム、アーティスト、歌詞などの情報は含まれていません。ただし、ユーザーがClovaアプリまたは画面のあるクライアントデバイスを使用している場合には、Custom Extensionは、クライアントがそのような情報を表示できるようにメタデータを提供する必要があります。

クライアントはオーディオコンテンツの再生メタデータを取得するために、[`TemplateRuntime.RequestPlayerInfo`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/TemplateRuntime.{{ book.DocMeta.FileExtensionForExternalLink }}#RequestPlayerInfo)イベント（クライアントからのリクエストを渡すためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）をClovaに送信します。そのとき、イベントの内容は[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージで、以下のように送信されます。ちなみに、以下のサンプルは`eJyr5lIqSSyITy4tKs4vUrJSUE`トークンを持つコンテンツを基準に、クライアントからその次の10曲のメタデータをリクエストしたことを表しています。
{% elif book.L10N.TargetCountryCode == "JP" %}
ユーザーのクライアントに[オーディオコンテンツの再生を指示する](#DirectClientToPlayAudio)[`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play)ディレクティブ（クライアントを制御するためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）には、タイトル、アルバム、アーティスト、歌詞などの情報は含まれていません。ただし、ユーザーがClovaアプリまたは画面のあるクライアントデバイスを使用している場合には、Custom Extensionは、クライアントがそのような情報を表示できるようにメタデータを提供する必要があります。

クライアントはオーディオコンテンツの再生メタデータを取得するために、[`TemplateRuntime.RequestPlayerInfo`](/Develop/References/CEK_API.md#RequestPlayerInfo)イベント（クライアントからのリクエストを渡すためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）をClovaに送信します。そのとき、イベントの内容は[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージで、以下のように送信されます。ちなみに、以下のサンプルは`eJyr5lIqSSyITy4tKs4vUrJSUE`トークンを持つコンテンツを基準に、クライアントからその次の10曲のメタデータをリクエストしたことを表しています。
{% endif %}

```json
{
  "context": {
    ...
  },
  "request": {
    "type": "EventRequest",
    "requestId": "e5464288-50ff-4e99-928d-4a301e083d41",
    "timestamp": "2017-09-05T05:41:21Z",
    "event": {
      "namespace": "TemplateRuntime",
      "name": "RequestPlayerInfo",
      "payload": {
        "token": "eJyr5lIqSSyITy4tKs4vUrJSUE",
        "range": {
          "after": 10
        }
      }
    }
  },
  "session": {
      "new": true,
      "sessionAttributes": {},
      "sessionId": "69b20cc1-9166-41f3-a2dd-85b70f8e0bf5"
  },
  "version": "1.0"
}
```

{% if book.L10N.TargetCountryCode == "KR" %}
Custom Extensionは、レスポンスメッセージを使って、クライアントからリクエストされたコンテンツのメタデータを返す必要があります。以下のように[`TemplateRuntime.RenderPlayerInfo`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/TemplateRuntime.{{ book.DocMeta.FileExtensionForExternalLink }}#RenderPlayerInfo)ディレクティブをレスポンスメッセージに含めて、メタデータを送信します。
{% elif book.L10N.TargetCountryCode == "JP" %}
Custom Extensionは、レスポンスメッセージを使って、クライアントからリクエストされたコンテンツのメタデータを返す必要があります。以下のように[`TemplateRuntime.RenderPlayerInfo`](/Develop/References/CEK_API.md#RenderPlayerInfo)ディレクティブをレスポンスメッセージに含めて、メタデータを送信します。
{% endif %}

```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "card": {},
    "directives": [
      {
        "header": {
          "namespace": "TemplateRuntime",
          "name": "RenderPlayerInfo"
        },
        "payload": {
          "controls": [
            {
              "enabled": true,
              "name": "PLAY_PAUSE",
              "selected": false,
              "type": "BUTTON"
            },
            {
              "enabled": true,
              "name": "NEXT",
              "selected": false,
              "type": "BUTTON"
            },
            {
              "enabled": true,
              "name": "PREVIOUS",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "displayType": "list",
          "playableItems": [
            {
              "artImageUrl": "https://musicmeta.musicservice.example.com/example/album/662058.jpg",
              "controls": [
                {
                  "enabled": true,
                  "name": "LIKE_DISLIKE",
                  "selected": false,
                  "type": "BUTTON"
                }
              ],
              "headerText": "Classic",
              "lyrics": [
                {
                  "data": null,
                  "format": "PLAIN",
                  "url": null
                }
              ],
              "isLive": false,
              "showAdultIcon": false,
              "titleSubText1": "Alice Sara Ott, Symphonie Orchester Des Bayerischen Rundfunks, Esa-Pekka Salonen",
              "titleSubText2": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces",
              "titleText": "Grieg : Piano Concerto In A Minor, Op.16 - 3. Allegro moderato molto e marcato (Live)",
              "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
            },
            {
              "artImageUrl": "https://musicmeta.musicservice.example.com/example/album/202646.jpg",
              "controls": [
                {
                  "enabled": true,
                  "name": "LIKE_DISLIKE",
                  "selected": false,
                  "type": "BUTTON"
                }
              ],
              "headerText": "Classic",
              "lyrics": [
                {
                  "data": null,
                  "format": "PLAIN",
                  "url": null
                }
              ],
              "isLive": true,
              "showAdultIcon": false,
              "titleSubText1": "Berliner Philharmoniker, Herbert Von Karajan",
              "titleSubText2": "Mendelssohn : Violin Concerto; A Midsummer Night`s Dream",
              "titleText": "Symphony No.4 In A Op.90 'Italian' - III.Con Moto Moderato",
              "token": "eJyr5lIqSSyITy4tKs4vUrJSUEo2"
            },
            ...
          ],
          "provider": {
            "logoUrl": "https://img.musicservice.example.net/logo_180125.png",
            "name": "SampleMusicProvider",
            "smallLogoUrl": "https://img.musicservice.example.net/smallLogo_180125.png"
          }
        }
      }
    ],
    "outputSpeech": {},
    "shouldEndSession": true
  }
}
```

## 再生状態の変更および進行状況のレポートを収集する {#CollectPlaybackStatusAndProgress}

{% if book.L10N.TargetCountryCode == "KR" %}
[`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play)ディレクティブ（クライアントを制御するためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）によってオーディオを再生するユーザーのクライアントは、再生を開始・一時停止・再開・停止・終了するタイミングで[`AudioPlayer.PlayStarted`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayStarted)、[`AudioPlayer.PlayPaused`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayPaused)、[`AudioPlayer.PlayResumed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayResumed)、[`AudioPlayer.PlayStopped`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayStopped)、[`AudioPlayer.PlayFinished`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayFinished)のようなイベント（クライアントからのリクエストを渡すためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）をClovaに送信します。そのとき、Clovaはそのイベントの内容を[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージでCustom Extensionに送信します。

また、クライアントは[オーディオコンテンツを再生するように指示（`AudioPlayer.Play`）](#DirectClientToPlayAudio)を受けた後、`AudioPlayer.Play`ディレクティブの`progressReport`フィールドに定義されている設定に従って再生の進行状況をレポートします。その内容もまた、[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージでCustom Extensionに送信されます。クライアントは、進行状況をレポートするために、以下のイベントを送信します。

* [`AudioPlayer.ProgressReportDelayPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportDelayPassed)イベント：再生が開始してから特定の時間が経過した後、再生の進行状況をレポートする
* [`AudioPlayer.ProgressReportPositionPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportPositionPassed)イベント：オーディオコンテンツの特定の位置（オフセット）を再生するときに、進行状況をレポートする
* [`AudioPlayer.ProgressReportIntervalPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportIntervalPassed)イベント：再生中に、特定の間隔で繰り返し進行状況をレポートする
{% elif book.L10N.TargetCountryCode == "JP" %}
[`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play)ディレクティブ（クライアントを制御するためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）によってオーディオを再生するユーザーのクライアントは、再生を開始・一時停止・再開・停止・終了するタイミングで[`AudioPlayer.PlayStarted`](/Develop/References/CEK_API.md#PlayStarted)、[`AudioPlayer.PlayPaused`](/Develop/References/CEK_API.md#PlayPaused)、[`AudioPlayer.PlayResumed`](/Develop/References/CEK_API.md#PlayResumed)、[`AudioPlayer.PlayStopped`](/Develop/References/CEK_API.md#PlayStopped)、[`AudioPlayer.PlayFinished`](/Develop/References/CEK_API.md#PlayFinished)のようなイベント（クライアントからのリクエストを渡すためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）をClovaに送信します。そのとき、Clovaはそのイベントの内容を[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージでCustom Extensionに送信します。

また、クライアントは[オーディオコンテンツを再生するように指示（`AudioPlayer.Play`）](#DirectClientToPlayAudio)を受けた後、`AudioPlayer.Play`ディレクティブの`progressReport`フィールドに定義されている設定に従って再生の進行状況をレポートします。その内容もまた、[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージでCustom Extensionに送信されます。クライアントは、進行状況をレポートするために、以下のイベントを送信します。

* [`AudioPlayer.ProgressReportDelayPassed`](/Develop/References/CEK_API.md#ProgressReportDelayPassed)イベント：再生が開始してから特定の時間が経過した後、再生の進行状況をレポートする
* [`AudioPlayer.ProgressReportPositionPassed`](/Develop/References/CEK_API.md#ProgressReportPositionPassed)イベント：オーディオコンテンツの特定の位置（オフセット）を再生するときに、進行状況をレポートする
* [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/CEK_API.md#ProgressReportIntervalPassed)イベント：再生中に、特定の間隔で繰り返し進行状況をレポートする
{% endif %}

以下は、`EventRequest`タイプのリクエストメッセージで送信されたレポートのサンプルです。
```json

{
  "context": {
    "AudioPlayer": {
      "offsetInMilliseconds": 60000,
      "playerActivity": "STOPPED",
      "stream": {
        "token": "TR-NM-17413540",
        "url": "https://music.serviceprovider.example.net/content?id=17413540",
        "urlPlayable": true
      },
      "totalInmillisecodns": 300000
    },
    "System": "{ ...}"
  },
  "request": {
    "type": "EventRequest",
    "requestId": "e5464288-50ff-4e99-928d-4a301e083d41",
    "timestamp": "2017-09-05T05:41:21Z",
    "event": {
      "namespace": "AudioPlayer",
      "name": "PlayStopped",
      "payload": {}
    }
  },
  "session": {
    "new": true,
    "sessionAttributes": {},
    "sessionId": "69b20cc1-9166-41f3-a2dd-85b70f8e0bf5"
  },
  "version": "1.0"
}
```

上記の`EventRequest`タイプのリクエストメッセージは、クライアントが全部で5分のオーディオコンテンツのうち、1分になるタイミングで再生を中断したことをレポートしています。Custom Extensionは、このようにして、クライアントの再生状態の変化を追跡することができます。例えば、`AudioPlayer.PlayStopped`と`AudioPlayer.PlayFinished`イベントが含まれた`EventRequest`タイプのリクエストメッセージを収集して、オーディオを最後まで聞いたり、または最後まで聞かないユーザーを区別し、それを統計データにすることができます。

また、`AudioPlayer.ProgressReportIntervalPassed`イベントが含まれた`EventRequest`タイプのリクエストメッセージを使って、完全に正確なわけではないけれど、ユーザーがオーディオコンテンツをどの位置まで聞いたかを把握することができます。ユーザーが次に同じオーディオコンテンツの再生をリクエストする場合、そのデータに基づいて、最後に聞いた位置から再生することができます。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>再生の進行状況のレポートに関する<code>EventRequest</code>タイプのリクエストメッセージのうち、<code>AudioPlayer.PlayFinished</code>イベントが含まれたメッセージを受信した場合、Custom Extensionは、再生完了に対するクライアントの次のアクションをレスポンスメッセージで返す必要があります。そのアクションとして、次の<a href="#DirectClientToPlayAudio">オーディオコンテンツの再生を指示する</a>こともできますし、再生停止などの<a href="#ControlAudioPlayback">再生コントロール</a>を指示することもできます。</p>
</div>

{% if book.L10N.TargetCountryCode == "KR" %}
ちなみに、このセクションで扱っている`AudioPlayer`名前空間イベントには、`AudioPlayer.PlaybackState`コンテキストが含まれます。その情報もまた、`EventRequest`タイプのリクエストメッセージが送信されるときに含まれるので、Custom Extensionは含まれた[`AudioPlayer.PlaybackState`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Context_Objects.{{ book.DocMeta.FileExtensionForExternalLink }}#PlaybackState)コンテキストからオーディオコンテンツのID、再生状態、オーディオコンテンツの再生位置などを把握できます。
{% elif book.L10N.TargetCountryCode == "JP" %}
ちなみに、このセクションで扱っている`AudioPlayer`名前空間イベントには、`AudioPlayer.PlaybackState`コンテキストが含まれます。その情報もまた、`EventRequest`タイプのリクエストメッセージが送信されるときに含まれるので、Custom Extensionは含まれた`AudioPlayer.PlaybackState`コンテキストからオーディオコンテンツのID、再生状態、オーディオコンテンツの再生位置などを把握できます。
{% endif %}

以下は、`AudioPlayer.PlaybackState`コンテキストが送信されたサンプルです。
```json
{
  "offsetInMilliseconds": 5077,
  "playerActivity": "PLAYING",
  "stream": {
    "token": "TR-NM-17413540",
    "url": "https://musicservice.example.net/content?id=17413540",
    "urlPlayable": true
  },
  "totalInMilliseconds": 195265
}
```

## セキュリティのためにオーディオコンテンツのURIを更新する {#UpdateAudioURIForSecurity}

{% if book.L10N.TargetCountryCode == "KR" %}
Custom Extensionがユーザーのクライアントに[オーディオコンテンツの再生を指示する](#DirectClientToPlayAudio)とき、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)に[`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play)ディレクティブ（クライアントを制御するためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）を含める必要があります。そのとき`AudioPlayer.Play`ディレクティブの`audioItem.stream.url`フィールドにオーディオコンテンツを再生できるURIを設定して送信します。
{% elif book.L10N.TargetCountryCode == "JP" %}
Custom Extensionがユーザーのクライアントに[オーディオコンテンツの再生を指示する](#DirectClientToPlayAudio)とき、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)に[`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play)ディレクティブ（クライアントを制御するためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）を含める必要があります。そのとき`AudioPlayer.Play`ディレクティブの`audioItem.stream.url`フィールドにオーディオコンテンツを再生できるURIを設定して送信します。
{% endif %}

ただしサービスの提供元によっては、セキュリティ上の問題で永久に有効なURIを含めない可能性があります。例えば、そのURIが漏えいされたらコンテンツを盗み取るための攻撃が発生する可能性があります。そのため、通常比較的に短い有効期限を持つインスタンスURIを使用します。また、クライアントが`AudioPlayer.Play`ディレクティブを受信していても、より優先順位の高いタスクや、先に開始したタスク、またはネットワークの状況によって、オーディオコンテンツの再生開始が遅延することがあります。その場合、URIの有効期限が切れてオーディオコンテンツを正しく再生できない可能性があります。

Clovaはクライアントがオーディオコンテンツを再生できるURIを再生直前に取得する方法で対応しています。最初に以下のような`AudioPlayer.Play`ディレクティブの`urlPlayable`フィールドを`false`に指定し、`url`フィールドにURIではない他の形式の値を設定します。

```json
{
  "audioItem": {
    "audioItemId": "9CPWU-c82302b2-ea29-4f6c-ba6e-20fd268d8c3b-c1570067",
    "title": "Symphony No.4 In A Op.90 'Italian' - III.Con Moto Moderato",
    "artist": "Unknown",
    "stream": {
      "beginAtInMilliseconds": 0,
      "progressReport": {
        "progressReportDelayInMilliseconds": null,
        "progressReportIntervalInMilliseconds": null,
        "progressReportPositionInMilliseconds": 60000
      },
      "token": "TR-NM-17413540",
      "url": "clova:TR-NM-17413540",
      "urlPlayable": false
    }
  },
  "playBehavior": "REPLACE_ALL"
}
```

{% if book.L10N.TargetCountryCode == "KR" %}
後でクライアントが`AudioPlayer.Play`ディレクティブを処理するとき、`urlPlayable`フィールドが`false`に指定されていると、有効なオーディオコンテンツのURIを取得するために[`AudioPlayer.StreamRequested`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#StreamRequested)イベント（クライアントからのリクエストを渡すためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）をClovaに送信します。そのとき、イベントの内容は[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージで、以下のように送信されます。
{% elif book.L10N.TargetCountryCode == "JP" %}
後でクライアントが`AudioPlayer.Play`ディレクティブを処理するとき、`urlPlayable`フィールドが`false`に指定されていると、有効なオーディオコンテンツのURIを取得するために[`AudioPlayer.StreamRequested`](/Develop/References/CEK_API.md#StreamRequested)イベント（クライアントからのリクエストを渡すためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）をClovaに送信します。そのとき、イベントの内容は[`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)タイプのリクエストメッセージで、以下のように送信されます。
{% endif %}

```json
{
  "context": {
    ...
  },
  "request": {
    "type": "EventRequest",
    "requestId": "e5464288-50ff-4e99-928d-4a301e083d41",
    "timestamp": "2017-09-05T05:41:21Z",
    "event": {
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      "payload": {
        "audioItemId": "9CPWU-c82302b2-ea29-4f6c-ba6e-20fd268d8c3b-c1570067",
        "title": "Symphony No.4 In A Op.90 'Italian' - III.Con Moto Moderato",
        "artist": "Unknown",
        "stream": {
          "beginAtInMilliseconds": 0,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17413540",
          "url": "clova:TR-NM-17413540",
          "urlPlayable": false
        }
      }
    }
  },
  "session": {
    "new": true,
    "sessionAttributes": {},
    "sessionId": "69b20cc1-9166-41f3-a2dd-85b70f8e0bf5"
  },
  "version": "1.0"
}
```

Custom Extensionは、そのタイミングで、再生できるオーディオコンテンツのURIを[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)で返す必要があります。そのために、`AudioPlayer.StreamDeliver`ディレクティブをレスポンスメッセージに含める必要があります。クライアントは、以下のような`AudioPlayer.StreamDeliver`ディレクティブのボディを用いて、`AudioPlayer.Play`ディレクティブを引き続き処理することができます。

```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "card": {},
    "directives": [
      {
        "header": {
          "namespace": "AudioPlayer",
          "name": "StreamDeliver"
        },
        "payload": {
          "audioItemId": "5313c879-25bb-461c-93fc-f85d95edf2a0",
          "stream": {
            "token": "b767313e-6790-4c28-ac18-5d9f8e432248",
            "url": "https://musicservice.example.net/b767313e.mp3"
          }
        }
      }
    ],
    "outputSpeech": {},
    "shouldEndSession": true
  }
}
```

## 再生制御の動作方法を変更する {#CustomizePlaybackControl}

{% if book.L10N.TargetCountryCode == "KR" %}
オーディオコンテンツを提供するサービスやコンテンツの特性によって、再生の一時停止、再生再開、再生停止などの[再生制御](#ControlAudioPlayback)の動作を少し違った形で実装する必要があることがあります。例えば、リアルタイムのストリーミングコンテンツは、一時停止機能を適用できない可能性があります。その場合、ユーザーから`Clova.PauseIntent`[ビルトインインテント](/Design/Design_Custom_Extension.md#BuiltinIntent)のリクエストがあっても、そのリクエストを処理できないと応答したり、または`Clova.StopIntent`のような対応を処理したりすることができます。`Clova.StopIntent`のような対応を処理する場合、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)に[`PlaybackController.Pause`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Pause)ディレクティブ（クライアントを制御するためのメッセージ、[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})）の代わりに[`PlaybackController.Stop`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Stop)ディレクティブを応答として返すように実装することができます。
{% elif book.L10N.TargetCountryCode == "JP" %}
オーディオコンテンツを提供するサービスやコンテンツの特性によって、再生の一時停止、再生再開、再生停止などの[再生制御](#ControlAudioPlayback)の動作を少し違った形で実装する必要があることがあります。例えば、リアルタイムのストリーミングコンテンツは、一時停止機能を適用できない可能性があります。その場合、ユーザーから`Clova.PauseIntent`[ビルトインインテント](/Design/Design_Custom_Extension.md#BuiltinIntent)のリクエストがあっても、そのリクエストを処理できないと応答したり、または`Clova.StopIntent`のような対応を処理したりすることができます。`Clova.StopIntent`のような対応を処理する場合、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)に[`PlaybackController.Pause`](/Develop/References/CEK_API.md#Pause)ディレクティブ（クライアントを制御するためのメッセージ、[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)）の代わりに[`PlaybackController.Stop`](/Develop/References/CEK_API.md#Stop)ディレクティブを応答として返すように実装することができます。
{% endif %}

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>ユーザーが混乱を感じないように、リアルタイムのストリーミングコンテンツなどの特殊な状況に限って再生コントロールの動作を変更し、なるべくデフォルトの実装をすることを推奨します。</p>
</div>
