# Checking the TTS playback status

When [returning a response in the custom extension](/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse), you can receive a progress report on the TTS playback from the client by inputting data in the `token` field of [SpeechInfoObject](/Develop/References/Custom_Extension_Message.html#CustomExtSpeechInfoObject) in the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage). This can be used to adjust the caption and its progress stage when the client displays subtitles or specific content.

For example, assume that the client displays "How can I help you?" after saying or displaying "Hi, my name is Clova." In this case, you must first send the audio for "Hi, my name is Clova" and only display the caption after the speech has ended.

Because there is no guarantee that the client will handle the action correctly, you must first receive a message from the client indicating that the audio has been played, and then send the subtitle and audio for the next message. To do this, you can send a token value for the phrase (TTS) using the `outputSpeech.values.token` following field as a response message.

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
The client can report the progress of the TTS playback to Clova using the appropriate [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }}) for each situation.

* [`SpeechSynthesizer.SpeechFinished`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#SpeechFinished) directive: Directs the client to pause the playing audio stream.
* [`SpeechSynthesizer.SpeechStarted`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#SpeechStarted) directive: Directs the client to resume the audio stream.
* [`SpeechSynthesizer.SpeechStopped`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#SpeechStopped) directive: Directs the client to stop the audio stream.
{% elif book.L10N.TargetCountryCode == "JP" %}
The client can report the progress of the TTS playback to Clova using the appropriate [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback) for each situation.

* [`SpeechSynthesizer.SpeechFinished`](/Develop/References/CEK_API.md#SpeechFinished) directive: Directs the client to pause the playing audio stream.
* [`SpeechSynthesizer.SpeechStarted`](/Develop/References/CEK_API.md#SpeechStarted) directive: Directs the client to resume the audio stream.
* [`SpeechSynthesizer.SpeechStopped`](/Develop/References/CEK_API.md#SpeechStopped) directive: Directs the client to stop the audio stream.
{% endif %}

Clova sends the report to the extension via an [`EventRequest` request type](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) message. The example below is a report that the audio has finished playing. After receiving this message, you can check the TTS playback status and adjust the speed or the steps of providing content by providing the proceeding audio or content as a response.

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
