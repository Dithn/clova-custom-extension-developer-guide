## Returning a custom extension response {#ReturnCustomExtensionResponse}
After [handling the request message](#HandleCustomExtensionRequest), you must return the HTTP [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) to CEK again. The details of the response may vary depending on the type of request message, but the structure of the response message does not vary much. Below is a response message sent after handling a LaunchRequest type request, where a user says "Start Pizzabot."

{% raw %}
```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "ko",
          "value": "Hello, I'm Pizzabot. What can I do for you?"
      }
    },
    "card": {},
    "directives": [],
    "shouldEndSession": false
  }
}
```
{% endraw %}

The fields represent the following information:

* `version`: The current version of the custom extension message format is v0.1.0.
* `response.outputSpeech`: The response "Hi, nice to meet you" is set to be said to the user in English.
* `response.card`: There is no data to display on the client screen. The data is in the [content template]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Content_Templates.{{ book.DocMeta.FileExtensionForExternalLink }}) format and can be displayed on the client screen via this field.
* `response.shouldEndSession`: The current session does not end and continuously receives user input. If the field value is set to true, the extension can end the session before receiving the [`SessionEndedRequest`](#HandleSessionEndedRequest) request.

<div class="tip">
  <p><strong>Tip!</strong></p>
  The <p><code>sessionAttributes</code> field is reserved for future use and the <code>response.directives</code> field is a directive message returned by the extension to CEK. The directive message API for the <code>response.directives</code> field will be provided in the future.</p>
</div>

The response message can be written to output multiple sentences based on the circumstances or to play audio or music files on the internet as shown below.

```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SpeechList",
      "values": [
        {
          "type": "PlainText",
          "lang": "ko",
          "value": "I will sing a song."
        },
        {
          "type": "URL",
          "lang": "" ,
          "value": "https://tts.example.com/song.mp3"
        }
      ]
    },
    "card": {},
    "directives": [],
    "shouldEndSession": true
  }
}
```

The `response.outputSpeech` fields represent the following information:

* `response.outputSpeech.type`: A complex type (SpeechList) of audio data.
* `response.outputSpeech.values[0]`: A regular text type of audio data. It has been set to say "I will sing a song" in Korean.
* `response.outputSpeech.values[1]`: A URI type of audio data. It has been set to play a file from the URI entered in the `value` field.

You can write this as follows when providing an HLS audio. At this time, you must designate the `response.outputSpeech.values[1].contentType` field as `"application/vnd.apple.mpegurl"`.

```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SpeechList",
      "values": [
        {
          "type": "PlainText",
          "lang": "ko",
          "value": "I will sing a song."
        },
        {
          "contentType": "application/vnd.apple.mpegurl",
          "type": "URL",
          "lang": "" ,
          "value": "https://tts.example.com/song.m3u8"
        }
      ]
    },
    "card": {},
    "directives": [],
    "shouldEndSession": true
  }
}
```

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>In addition to simple or complex sentence type of audio data, we support a combined format (SpeechSet) for client devices with limitations for expressing detailed GUIs such as devices without a display. For more information, see <a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">Response message</a> in the custom extension message format.</p>
</div>

To display data on the screen of a client device or Clova app in addition to outputting audio, fill in the content in the `response.card` field in accordance with the [content template]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Content_Templates.{{ book.DocMeta.FileExtensionForExternalLink }}).

{% raw %}
```json
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "ko",
          "value": "Here is a picture of Lionel Messi."
      }
    },
    "card": {
      "type": "ImageText",
      "imageUrl": {
        "type": "url",
        "value": ""
      },
      "mainText": {
        "type": "string",
        "value": "Lionel Messi"
      },
      "referenceText": {
        "type": "string",
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.example.com/search.naver?where=m&sm=mob_lic&query=%eb%a6%ac%ec%98%a4%eb%84%ac+%eb%a9%94%ec%8b%9c+%ec%86%8c%ec%86%8d%ed%8c%80"
      },
      "subTextList": [
        {
          "type": "string",
          "value": "FC Barcelona"
        }
      ],
      "thumbImageType": {
        "type": "string",
        "value": "People"
      },
      "thumbImageUrl": {
        "type": "url",
        "value": "https://sstatic.example.net/people/3/201607071816066361.jpg"
      }
    },
    "directives": [],
    "shouldEndSession": true
  }
}
```
{% endraw %}
