# 音声の再生状態を確認する

[Custom Extensionからレスポンスを返す](/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse)とき、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)の[SpeechInfoObject](/Develop/References/CEK_API.md#CustomExtSpeechInfoObject)の`token`フィールドを入力すると、クライアントから合成音声（TTS）再生の進行状況のレポートを受信します。主に、クライアントが字幕や特定のコンテンツを表示するとき、音声の字幕とその進行状況を合わせるために使用します。

例えば、「Hi, my name is Clova」という音声とテキストを表示し、続けて「How can I help you?」というテキストを表示する場合を仮定します。そのとき、「Hi, my name is Clova」に該当する音声を再生するように送信した後、その音声の再生が完了してから、次の字幕が表示されるようにする必要があります。

クライアントがそのアクションを正確に処理するという保証がないため、クライアントから1番目の音声が再生されたというレポートを受信してから、その次の音声に該当する字幕と音声を送信する必要があります。そのために、1番目の音声に対するレスポンスメッセージとして、次のように`outputSpeech.values.token`フィールドを使用し、合成音声（TTS）のトークンを渡します。

```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "en",
          "token": "19d33bae-6cd5-4534-b0a3-e0036b4742bd",
          "value": "Hi, my name is Clova."
      }
    },
    "card": {},
    "directives": [
      {
        "directive": {
          "header": {
            "namespace": "Clova",
            "name": "RenderTemplate",
          },
          "payload": {
            {
              ...
              "paragraphText": {
                "type": "string",
                "value": "Hi, my name is Clova."
              },
              ...
              },
              "type": "Text"
            }
          }
        }
      }
    ],
    "shouldEndSession": false
  }
}
```

{% if book.L10N.TargetCountryCode == "KR" %}
クライアントは、状況に応じて適切な[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.md)を使用して、Clovaに合成音声再生の進行状況をレポートします。

* [`SpeechSynthesizer.SpeechFinished`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechFinished)ディレクティブ：クライアントに、再生中のオーディオストリームを一時停止するように指示する
* [`SpeechSynthesizer.SpeechStarted`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechStarted)ディレクティブ：クライアントに、オーディオストリームの再生を再開するように指示する
* [`SpeechSynthesizer.SpeechStopped`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechStopped)ディレクティブ：クライアントに、オーディオストリームの再生を停止するように指示する
{% elif book.L10N.TargetCountryCode == "JP" %}
クライアントは、状況に応じて適切な[`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)を使用して、Clovaに合成音声再生の進行状況をレポートします。

* [`SpeechSynthesizer.SpeechFinished`](/Develop/References/CEK_API.md#SpeechFinished)ディレクティブ：クライアントに、再生中のオーディオストリームを一時停止するように指示する
* [`SpeechSynthesizer.SpeechStarted`](/Develop/References/CEK_API.md#SpeechStarted)ディレクティブ：クライアントに、オーディオストリームの再生を再開するように指示する
* [`SpeechSynthesizer.SpeechStopped`](/Develop/References/CEK_API.md#SpeechStopped)ディレクティブ：クライアントに、オーディオストリームの再生を停止するように指示する
{% endif %}

Clovaは、そのレポートを[`EventRequest`リクエストタイプ](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)のメッセージでExtensionに送信します。以下は、上記の音声に対して、再生終了のレポートを受信したサンプルです。以下のメッセージを受信すると、クライアントの現在の音声の再生状態を確認し、次に送信する音声やコンテンツをレスポンスとして提供することで、コンテンツを提供するスピードや段階を調節します。

```json
{
  "version": "0.1.0",
  "session": {
    ...
  },
  "context": {
    ...
  },
  "request": {
    "type": "EventRequest",
    "requestId": "cdb56a98-e940-4ee4-96eb-4f1cb3f97694",
    "timestamp": "2017-09-05T05:41:21Z",
    "event": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechFinished",
      "payload": {
        "token": "19d33bae-6cd5-4534-b0a3-e0036b4742bd"
      }
    }
  }
}
```
