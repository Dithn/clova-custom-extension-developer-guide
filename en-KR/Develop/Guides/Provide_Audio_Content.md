# Providing audio content

{% if book.L10N.TargetCountryCode == "KR" %}
You can use the custom extension to provide audio content such as music or podcasts to users. To do this, you must use an [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type request message of the [custom extension message](/Develop/References/Custom_Extension_Message.md) and the [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }}) related to the audio content playback, which is the message format of the client (user device or Clova app) from the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) specification. To provide audio content to users, you must implement the following details on the extension. The details are covered in each section. **In particular, all mandatory items must be implemented.**

* Mandatory implementations
  * [Directing audio content playback](#DirectClientToPlayAudio)
  * [Controlling audio content playback](#ControlAudioPlayback)
  * [Providing audio content metadata for display](#ProvidingMetaDataForDisplay)

* Optional implementations
  * [Collecting changes in playback state and progress reports](#CollectPlaybackStatusAndProgress)
  * [Updating audio content URI for security](#UpdateAudioURIForSecurity)
  * [Customizing playback control](#CustomizePlaybackControl)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>To implement a custom extension that plays audio content, you must set the {{ book.DevConsole.cek_audioplayer }} item to <strong>Yes</strong> in the <a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">basic information</a> when you <a href="/DevConsole/Guides/Register_Custom_Extension.md">register the extension on the Clova developer console</a>.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>It is recommended to use the HTTPS method when you provide TTS or audio content as an audio file (.mp3) using the custom extension. Although the Clova platform currently also supports audio files provided using the HTTP method, restrictions may apply anytime that require audio files to have a URI in the HTTPS method.</p>
</div>
{% elif book.L10N.TargetCountryCode == "JP" %}
You can use the custom extension to provide audio content such as music or podcasts to users. To do this, you must use the [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type request message of the [custom extension messages](/Develop/References/Custom_Extension_Message.md) and the [CIC API](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)  related to the audio content playback which is the message format for client (user device or Clova app) from the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) specification. To provide audio content to users, you must implement the following details on the extension. The details are covered in each section.  **In particular, all mandatory items must be implemented.**

* Mandatory implementations
  * [Directing audio content playback](#DirectClientToPlayAudio)
  * [Controlling audio content playback](#ControlAudioPlayback)
  * [Providing audio content metadata for display](#ProvidingMetaDataForDisplay)

* Optional implementations
  * [Collecting changes in playback state and progress reports](#CollectPlaybackStatusAndProgress)
  * [Updating audio content URI for security](#UpdateAudioURIForSecurity)
  * [Customizing playback control](#CustomizePlaybackControl)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>To implement a custom extension that plays audio content, you must set the {{ book.DevConsole.cek_audioplayer }} item to <strong>Yes</strong> in the <a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">basic information</a> when you <a href="/DevConsole/Guides/Register_Custom_Extension.md">register the extension on the Clova developer console</a>.</p>
</div>
{% endif %}

## Directing audio content playback {#DirectClientToPlayAudio}

{% if book.L10N.TargetCountryCode == "KR" %}
If the user requests the client to play audio content or play in a musical style, you must send this audio content to the user client. The audio playback request is sent to the custom extension as an [`IntentRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtIntentRequest) type request message and the custom extension must send the corresponding [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) for this `IntentRequest` type request message. For this process, include the [`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play) directive message (client control message, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) that directs the client to play the audio content.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The provided audio content must be in the <a href="/Design/Supported_Audio_Format.md">platform-supported audio compression format</a>.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Details on playback control is the main feature and a mandatory item of the custom extension providing audio content.</p>
</div>

Check the specification of the [`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play) directive message and include the audio information to be sent to the client in the custom extension response message as shown below. The fields or audio information that need to be filled out may vary depending on the information or characteristics of the audio in possession.
{% elif book.L10N.TargetCountryCode == "JP" %}
If the user requests the client to play audio content or play in a musical style, you must send this audio content to the user client. The audio playback request is sent to the custom extension as an [`IntentRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtIntentRequest) type request message and the custom extension must send the corresponding [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) for this `IntentRequest` type request message. For this process, include the [`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play) directive message (client control message, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) that directs the client to play the audio content.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The provided audio content must be in the <a href="/Design/Design_Guideline_For_Client_Hardware.md#SupportedAudioFormat">platform-supported audio compression format</a>.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Details on playback control is the main feature and a mandatory item of the custom extension providing audio content.</p>
</div>

Check the specification of the [`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play) directive message and include the audio information to be sent to the client in the custom extension response message as shown below. The fields or audio information that need to be filled out may vary depending on the information or characteristics of the audio in possession.
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
  <p><strong>Tip!</strong></p>
  <p>When <a href="/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse">returning the response message</a>, you can also specify the <code>response.outputSpeech</code> field. For example, you can first output speech (TTS) saying, "Here is the requested audio" to the user then start playing the audio.</p>
</div>

{% if book.L10N.TargetCountryCode == "KR" %}
<div class="note">
  <p><strong>Note!</strong></p>
  <p>When writing the <a href="{{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }}">CIC API</a> directive message</p>, the field with "inclusion status" of <strong>"always"</strong> is a required field and must be filled out.</p>
</div>
{% elif book.L10N.TargetCountryCode == "JP" %}
<div class="note">
  <p><strong>Note!</strong></p>
  <p>When writing the <a href="/Develop/References/CEK_API.md#CICAPIforAudioPlayback">CIC API</a> directive message</p>, the field with "inclusion status" of <strong>"always"</strong> is a required field and must be filled out.</p>
</div>
{% endif %}

## Controlling audio content playback {#ControlAudioPlayback}
If the user makes an utterance related to playback controls, such as "Previous" or "Next," when the user client is playing audio content, the user request can be sent to the custom extension as an `IntentRequest` type request message. Currently, CEK is sending the user intents for playback controls to the custom extension as the following [built-in intent](/Design/Design_Custom_Extension.md#BuiltinIntent):

* `Clova.NextIntent`
* `Clova.PauseIntent`
* `Clova.PreviousIntent`
* `Clova.RequestAlternativesIntent`
* `Clova.ResumeIntent`
* `Clova.StopIntent`

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Items related to playback controls are subject to mandatory implementation. Particularly, not implementing the actions that respond to the <code>Clova.PauseIntent</code> and the <code>Clova.StopIntent</code> built-in intents may result in serious inconvenience to users.</p>
</div>

{% if book.L10N.TargetCountryCode == "KR" %}
If the user makes an utterance such as "Pause," "Play again," or "Stop," the custom extension must respond appropriately to each pause, resume play, or cancel play request. In this process, the client receives `Clova.PauseIntent`, `Clova.ResumeIntent`, and `Clova.StopIntent` built-in intents as an `IntentRequest` type request message on each request. The custom extension must send the following directive messages (client control message, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) to CEK as [response messages](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage).

* [`PlaybackController.Pause`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Pause) directive: Directs the client to pause the playing audio stream.
* [`PlaybackController.Resume`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Resume) directive: Directs the client to resume the audio stream.
* [`PlaybackController.Stop`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Stop) directive: Directs the client to stop the audio stream.
{% elif book.L10N.TargetCountryCode == "JP" %}
If the user makes an utterance such as "Pause," "Play again," or "Stop," the custom extension must respond appropriately to each pause, resume play, or cancel play request. In this process, the client receives `Clova.PauseIntent`, `Clova.ResumeIntent`, and `Clova.StopIntent` built-in intents as an `IntentRequest` type request message on each request. The custom extension must send the following directive messages (client control message, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) to CEK as [response messages](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage).

* [`PlaybackController.Pause`](/Develop/References/CEK_API.md#Pause) directive: Directs the client to pause the playing audio stream.
* [`PlaybackController.Resume`](/Develop/References/CEK_API.md#Resume) directive: Directs the client to resume the audio stream.
* [`PlaybackController.Stop`](/Develop/References/CEK_API.md#Stop) directive: Directs the client to stop the audio stream.
{% endif %}

Here is an example of including the `PlaybackController.Pause` directive message in the response message of the custom extension.
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

When the client receives the `Clova.NextIntent` or `Clova.PreviousIntent` built-in intent as an `IntentRequest` type request message after the user makes an utterance corresponding to "Previous" or "Next," write a [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) to [direct the client to play the audio content(`AudioPlayer.Play`)](#DirectClientToPlayAudio) falling under previous or next song of the current playlist.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>If there is no valid audio content to play, output speech like "There is no previous/next song to play" as a <a href="/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse">response message</a>.</p>
</div>

If the user made an utterance corresponding to "Play another one"(`Clova.RequestAlternativesIntent`), respond by making judgment of the situation. Response types can be diverse, and below are examples of responding to the `Clova.RequestAlternativesIntent` built-in intent.
* Write a [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) to [direct the client to play the audio content(`AudioPlayer.Play`)](#DirectClientToPlayAudio) of a brand new playlist.
* Write a response message to direct the client to play a similar spot.
* Write a response message to direct the client to play the next song.
* [Do multi-turn dialog](/Develop/Guides/Do_Multiturn_Dialog.md) to specifically identify what the user wants.
* [Provide TTS](/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse) when there is no more content to provide or when it is impossible to grant the user request.


## Providing audio content metadata for display {#ProvidingMetaDataForDisplay}

{% if book.L10N.TargetCountryCode == "KR" %}
In the [`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play) directive message (client control message, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) that [directs the user client to play audio content](#DirectClientToPlayAudio), information such as title, album, artist, or lyrics are not included. However, if the user is using the Clova app or a client device with a screen, the custom extension must provide metadata to display such information.

For this, the client sends the [`TemplateRuntime.RequestPlayerInfo`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/TemplateRuntime.{{ book.DocMeta.FileExtensionForExternalLink }}#RequestPlayerInfo) event message (message for sending a client request, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) to Clova to obtain the playback metadata of audio content. For this process, details of the event message are sent as a request message of the [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type as shown below. Note that the example below refers to a client request of metadata on the next 10 songs, based on the content that has the `eJyr5lIqSSyITy4tKs4vUrJSUE` token.
{% elif book.L10N.TargetCountryCode == "JP" %}
In the [`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play) directive message (client control message, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) that [directs the user client to play audio content](#DirectClientToPlayAudio), information such as title, album, artist, or lyrics are not included. However, if the user is using the Clova app or a client device with a screen, the custom extension must provide metadata to display such information.

For this, the client sends the [`TemplateRuntime.RequestPlayerInfo`](/Develop/References/CEK_API.md#RequestPlayerInfo) event message (message for sending a client request, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) to Clova to obtain the playback metadata on audio contents. For this process, details of the event message are sent as a request message of the [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type as shown below. Note that the example below refers to a client request of metadata on the next 10 songs, based on the content that has the `eJyr5lIqSSyITy4tKs4vUrJSUE` token.
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
The custom extension must send the metadata of the content which the client has requested using a response message. Send metadata by including the [`TemplateRuntime.RenderPlayerInfo`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/TemplateRuntime.{{ book.DocMeta.FileExtensionForExternalLink }}#RenderPlayerInfo) directive message in the response message as shown below.
{% elif book.L10N.TargetCountryCode == "JP" %}
The custom extension must send the metadata of the content which the client has requested using a response message. Send metadata by including the [`TemplateRuntime.RenderPlayerInfo`](/Develop/References/CEK_API.md#RenderPlayerInfo) directive message in the response message as shown below.
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
              "titleText": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
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

## Collecting changes in playback state and progress reports {#CollectPlaybackStatusAndProgress}

{% if book.L10N.TargetCountryCode == "KR" %}
The user client playing audio content by the [`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play) directive message (message for client control, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) sends event messages, such as [`AudioPlayer.PlayStarted`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayStarted), [`AudioPlayer.PlayPaused`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayPaused), [`AudioPlayer.PlayResumed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayResumed), [`AudioPlayer.PlayStopped`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayStopped), and [`AudioPlayer.PlayFinished`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayFinished) (message for sending client request [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})), when playing has started, paused, resumed, stopped, or finished. In this process, Clova sends the details of this event message to the custom extension as an [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type request message.

Also, the client gets [directed to play the audio content (`AudioPlayer.Play`)](#DirectClientToPlayAudio) and reports on the playback progress according to the settings defined in the `progressReport` field of the `AudioPlayer.Play` directive message. This information also gets sent to the custom extension as an [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type request message. The client sends the following event messages for progress reporting:

* [`AudioPlayer.ProgressReportDelayPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportDelayPassed) event: Reports playback progress after a set time elapses once playing starts.
* [`AudioPlayer.ProgressReportPositionPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportPositionPassed) event: Reports progress when playing a specific offset of the audio content.
* [`AudioPlayer.ProgressReportIntervalPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportIntervalPassed) event: Reports progress at every set interval during playback.
{% elif book.L10N.TargetCountryCode == "JP" %}
The client playing audio content by the [`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play) directive message (message for client control, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) sends event messages such as [`AudioPlayer.PlayStarted`](/Develop/References/CEK_API.md#PlayStarted), [`AudioPlayer.PlayPaused`](/Develop/References/CEK_API.md#PlayPaused), [`AudioPlayer.PlayResumed`](/Develop/References/CEK_API.md#PlayResumed), [`AudioPlayer.PlayStopped`](/Develop/References/CEK_API.md#PlayStopped), and [`AudioPlayer.PlayFinished`](/Develop/References/CEK_API.md#PlayFinished) (message for sending client request, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) when playing has started, paused, resumed, stopped, or finished. In this process, Clova sends the details of this event message to the custom extension as an [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type request message.

Also, the client gets [directed to play the audio content (`AudioPlayer.Play`)](#DirectClientToPlayAudio) and reports on the playback progress according to the settings defined in the `progressReport` field of the `AudioPlayer.Play` directive message. This information also gets sent to the custom extension as an [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type request message. The client sends the following event messages for progress reporting:

* [`AudioPlayer.ProgressReportDelayPassed`](/Develop/References/CEK_API.md#ProgressReportDelayPassed) event: Reports playback progress after a set time elapses once playing starts.
* [`AudioPlayer.ProgressReportPositionPassed`](/Develop/References/CEK_API.md#ProgressReportPositionPassed) event: Reports progress when playing a specific offset of the audio content.
* [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/CEK_API.md#ProgressReportIntervalPassed) event: Reports progress at every set interval during playback.
{% endif %}

Here is an example of a report sent through an `EventRequest` type request message.
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
    "System": "{ ... }"
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

The above `EventRequest` type request message reports that the client has stopped playing after one minute has elapsed in a five minute piece of audio content. Like this, the custom extension can track the change of playback state on the client. For example, the custom extension can collect the `EventRequest` type request message containing `AudioPlayer.PlayStopped` and `AudioPlayer.PlayFinished` event message information to identify users who finish or do not finish listening to the audio content, and use this information to create statistics.

Also, you can use the `EventRequest` type request message which includes the `AudioPlayer.ProgressReportIntervalPassed` event message to understand approximately where the user has stopped listening to the audio content. If the user requests to play the same audio content next time, this data can be used to continue playing the content from where the playback last left off.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the custom extension receives a message that includes the <code>AudioPlayer.PlayFinished</code> event message information among the <code>EventRequest</code> type request messages related to the playback state report, the custom extension must send the next action of the client related to the completion of playback as a response message. The action can be <a href="#DirectClientToPlayAudio">directing the client to play audio content</a> or directing <a href="#ControlAudioPlayback">playback control</a> such as cancel play.</p>
</div>

{% if book.L10N.TargetCountryCode == "KR" %}
Note that `AudioPlayer.PlaybackState` context is attached to the `AudioPlayer` namespace event message mentioned in this section. Since this information is also attached when sending an `EventRequest` type request message, the custom extension can identify information, such as audio content ID, playback state, or the audio content offset, from the attached [`AudioPlayer.PlaybackState`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Context_Objects.{{ book.DocMeta.FileExtensionForExternalLink }}#PlaybackState) context information.
{% elif book.L10N.TargetCountryCode == "JP" %}
Note that `AudioPlayer.PlaybackState` context is attached to the `AudioPlayer` namespace event message mentioned in this section. Since this information is also attached when sending an `EventRequest` type request message, the custom extension can identify information, such as audio content ID, playback state, or the audio content offset, from the attached `AudioPlayer.PlaybackState` context information.
{% endif %}

Here is an example of sending the `AudioPlayer.PlaybackState` context information.
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

## Updating audio content URI for security {#UpdateAudioURIForSecurity}

{% if book.L10N.TargetCountryCode == "KR" %}
When the custom extension [directs the user client to play audio content](#DirectClientToPlayAudio), the [`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#Play) directive message (client control message, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) must be included in the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage). In this process, the URI to play the audio content is sent in the `audioItem.stream.url` field of the `AudioPlayer.Play` directive message.
{% elif book.L10N.TargetCountryCode == "JP" %}
When the custom extension [directs the user client to play audio content](#DirectClientToPlayAudio), the [`AudioPlayer.Play`](/Develop/References/CEK_API.md#Play) directive message (client control message, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) must be included in the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage). In this process, the URI to play the audio content is sent in the `audioItem.stream.url` field of the `AudioPlayer.Play` directive message.
{% endif %}

However, it may not be possible to attach a permanently valid URI depending on the service provider due to security issues. One of the reasons is that if this URI is exposed, a hacking attempt may occur to steal the content. Therefore, it is common to use an instance URI with a relatively short expiration date. Also, playing audio content may be delayed due to a task with higher priority, that has started earlier, or due to the network state even if the client has received the `AudioPlayer.Play` directive message. At this time, the URI may expire so audio content playback may not be possible.

Due to this reason, Clova provides a method for the client to acquire the URI for audio content playback right before playing. To do so, first, specify the `urlPlayable` field as `false` in the `AudioPlayer.Play` directive message and enter a value that is not in a URI format in the `url` field.

```json
{
  "audioItem": {
    "audioItemId": "9CPWU-c82302b2-ea29-4f6c-ba6e-20fd268d8c3b-c1570067",
    "title": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
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
If the `urlPlayable` field is `false` when the client executes the `AudioPlayer.Play` directive message, the [`AudioPlayer.StreamRequested`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#StreamRequested) event message (message for sending a client request, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) is sent to Clova to obtain a URI for valid audio content. For this process, details of the event message are sent as a request message of the [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type as shown below.
{% elif book.L10N.TargetCountryCode == "JP" %}
If the `urlPlayable` field is `false` when the client executes the `AudioPlayer.Play` directive message, the [`AudioPlayer.StreamRequested`](/Develop/References/CEK_API.md#StreamRequested) event message (message for sending a client request, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) is sent to Clova to obtain a URI for a valid audio content. For this process, details of the event message are sent as a request message of the [`EventRequest`](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest) type as shown below.
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
        "title": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
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

The custom extension can then send the URI of the audio content that can be played as a [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage). For this, you must include the `AudioPlayer.StreamDeliver` directive message in the response message. The client can perform the remaining task of the `AudioPlayer.Play` directive message via the body of the `AudioPlayer.StreamDeliver` directive message as follows:

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

## Customizing playback controls {#CustomizePlaybackControl}

{% if book.L10N.TargetCountryCode == "KR" %}
You may need to implement the [control audio playback](#ControlAudioPlayback) actions, such as pause, resume play, or cancel play, in different ways, depending on the service that provides audio content or the characteristics of the audio content. For example, it may not be possible to apply the pause function to real-time streaming content. At this time, upon receiving the `Clova.PauseIntent` [built-in intent](/Design/Design_Custom_Extension.md#BuiltinIntent) request from the user, you can indicate that the request cannot be handled or use intents like `Clova.StopIntent` as a response. To handle a response like `Clova.StopIntent`, you can make the extension to return the [`PlaybackController.Stop`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Stop) directive message as a response instead of the [`PlaybackController.Pause`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/PlaybackController.{{ book.DocMeta.FileExtensionForExternalLink }}#Pause) directive message (client control message, [CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})) to the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage).
{% elif book.L10N.TargetCountryCode == "JP" %}
You may need to implement the [control audio playback](#ControlAudioPlayback) actions, such as pause, resume play, or cancel play, in different ways, depending on the service that provides audio content or the characteristics of the audio content. For example, it may not be possible to apply the pause function to real-time streaming content. At this time, upon receiving the `Clova.PauseIntent` [built-in intent](/Design/Design_Custom_Extension.md#BuiltinIntent) request from the user, you can indicate that the request cannot be handled or use intents like `Clova.StopIntent` as a response. To handle a response like `Clova.StopIntent`, you can make the extension to return the [`PlaybackController.Stop`](/Develop/References/CEK_API.md#Stop) directive message as a response instead of the [`PlaybackController.Pause`](/Develop/References/CEK_API.md#Pause) directive message (client control message, [`CIC API`](/Develop/References/CEK_API.md#CICAPIforAudioPlayback)) to the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage).
{% endif %}

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>To avoid user confusion, it is highly recommended that you use the default method and only use the customized playback controls in special situations such as for real-time content streaming.</p>
</div>
