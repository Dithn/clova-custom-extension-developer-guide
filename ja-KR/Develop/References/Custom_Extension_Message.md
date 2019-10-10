<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Custom Extensionのメッセージ
Custom Extensionのメッセージは、CEKとCustom Extensionが情報をやり取りする際に使用されるメッセージです。Custom Extensionのメッセージは、[リクエストメッセージ](#CustomExtRequestMessage)と[レスポンスメッセージ](#CustomExtResponseMessage)の2種類があります。リクエストメッセージには、[リクエストタイプ](#CustomExtRequestType)によって`EventRequest`、`IntentRequest`、`LaunchRequest`、`SessionEndedRequest`の4つのタイプがあります。

## リクエストメッセージ {#CustomExtRequestMessage}
CEKは、Clovaが解析したユーザーのリクエストをCustom Extensionに渡すために、リクエストメッセージを送信します（HTTPリクエスト）。ここでは、リクエストメッセージの構造、各フィールドの説明、リクエストタイプとそれによって異なる`request`フィールドについて説明します。

### Message structure

{% raw %}
```json
{
  "context": {
    "AudioPlayer": {
      "offsetInMilliseconds": {{number}},
      "playerActivity": {{string}},
      "stream": {{AudioStreamInfoObject}},
      "totalInMilliseconds": {{number}},
    },
    "System": {
      "application": {
        "applicationId": {{string}}
      },
      "device": {
        "deviceId": {{string}},
        "display": {
          "contentLayer": {
            "width": {{number}},
            "height": {{number}}
          },
          "dpi": {{number}},
          "orientation": {{string}},
          "size": {{string}}
        }
      },
      "user": {
        "userId": {{string}},
        "accessToken": {{string}}
      }
    }
  },
  "request": {{object}},
  "session": {
    "new": {{boolean}},
    "sessionAttributes": {{object}},
    "sessionId": {{string}},
    "user": {
      "userId": {{string}},
      "accessToken": {{string}}
    }
  },
  "version": {{string}}
}
```
{% endraw %}

### Message fields
| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `context`                                  | object  | クライアントのコンテキスト情報を持っているオブジェクト                                |  |
| `context.AudioPlayer`                      | object  | クライアントが現在再生しているか、最後に再生したメディアの情報を持っているオブジェクト | 条件付き |
| `context.AudioPlayer.offsetInMilliseconds` | number  | 最近再生したメディアアイテムの最後の再生ポイント（オフセット）。ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドが空であることがあります。                                       | 条件付き |
| `context.AudioPlayer.playerActivity`       | string  | プレイヤーの状態を示す値です。次のような値を持ちます。<ul><li><code>"IDLE"</code>：非アクティブ状態</li><li><code>"PLAYING"</code>：再生中</li><li><code>"PAUSED"</code>：一時停止状態</li><li><code>"STOPPED"</code>：停止状態</li></ul> |  |
| `context.AudioPlayer.stream`               | [AudioStreamInfoObject]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#AudioStreamInfoObject) | 再生中のオーディオの詳細情報が保存されているオブジェクト`playerActivity`の値が`"IDLE"`の場合、このフィールドが空であることがあります。    | 条件付き |
| `context.AudioPlayer.totalInMilliseconds`  | number  | 最近再生したメディアの全長ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドが空であることがあります。                                                                  | 条件付き |
| `context.System`                           | object  | クライアントシステムのコンテキスト情報を持っているオブジェクト                          |  |
| `context.System.application`               | object  | ユーザーの意図によって実行されるExtensionの情報を持っているオブジェクト       |  |
| `context.System.application.applicationId` | string  | ExtensionのID                                                 |  |
| `context.System.device`                    | object  | クライアントデバイスの情報を持っているオブジェクト                               |  |
| `context.System.device.deviceId`           | string  | クライアントデバイスのID。モデル名とデバイスのシリアル番号を組み合わせた情報など、ユーザーのデバイスを識別できる情報が渡されます。 |  |
| `context.System.device.display`            | object  | クライアントデバイスのディスプレイ情報を持っているオブジェクト                                                 |  |
| `context.System.device.display.contentLayer`        | object | ディスプレイでコンテンツが表示される領域の解像度情報を持つオブジェクト。`context.System.device.display.size`の値が`"none"`の場合、このフィールドは省略されます。  | 条件付き |
| `context.System.device.display.contentLayer.width`  | number | ディスプレイでコンテンツが表示される領域の幅。ピクセル（px）単位です。             |  |
| `context.System.device.display.contentLayer.height` | number | ディスプレイでコンテンツが表示される領域の高さ。ピクセル（px）単位です。             |  |
| `context.System.device.display.dpi`         | number | ディスプレイ装置のDPI。`context.System.device.display.size`の値が`"none"`の場合、このフィールドは省略されます。          | 条件付き |
| `context.System.device.display.orientation` | string | ディスプレイ装置の方向。`context.System.device.display.size`の値が`"none"`の場合、このフィールドは省略されます。<ul><li><code>"landscape"</code>：横方向</li><li><code>"portrait"</code>：縦方向</li></ul>                      | 条件付き |
| `context.System.device.display.size`        | string | ディスプレイ装置の解像度のサイズを示す値。あらかじめ指定された値または任意の解像度のサイズを表す値（`"custom"`）が入力されていることがあります。または、ディスプレイ装置がないことを示す値（`"none"`）が入力されていることもあります。<ul><li><code>"none"</code>：クライアントデバイスにディスプレイ装置がない</li><li><code>"s100"</code>：低解像度（160px X 107px）</li><li><code>"m100"</code>：中解像度（427px X 240px）</li><li><code>"l100"</code>：高解像度（640px X 360px）</li><li><code>"xl100"</code>：超高解像度（xlarge type、899px X 506px）</li><li><code>"custom"</code>：あらかじめ定義された規格ではない解像度。</li></ul><div class="note"><p><strong>メモ</strong></p><p>クライアントデバイスのアスペクト比とDPIに適合した画質のメディアコンテンツを提供する必要があります。</p></div> |  |
| `context.System.user`                      | object  | クライアントデバイスに認証されたのデフォルトユーザーの情報を持っているオブジェクト                 |  |
| `context.System.user.userId`               | string  | デバイスのデフォルトユーザーのClova ID                                    |  |
| `context.System.user.accessToken`          | string  | 特定のサービスのユーザーアカウントのアクセストークン。デバイスのデフォルトユーザーと関連付けられたユーザーアカウントのアクセストークンが渡されます。CEKは、外部サービスの認可サーバーから取得したユーザーアカウントのアクセストークンを渡します。詳細については、[ユーザーアカウントを連携する](/Develop/Guides/Link_User_Account.md)を参照してください。 |  |
| `request`                                 | object  | 解析されたユーザーの発話情報を持っているオブジェクト。[リクエストタイプ](#CustomExtRequestType)によって、構成されるフィールドが異なります。 |  |
| `session`                                  | object  | セッション情報を持っているオブジェクトここでセッションとは、ユーザーのリクエストを区分する論理的な単位です。     |  |
| `session.new`                              | boolean | リクエストメッセージが新しいセッションに対するものか、それとも既存のセッションに対するものかを区分します。<ul><li>true：新しいセッション</li><li>false：既存のセッション</li></ul>  |  |
| `session.sessionAttributes`                       | object  | ユーザーとのマルチターン対話に必要な情報を保存したオブジェクト。Custom Extensionは、[レスポンスメッセージ](#CustomExtResponseMessage)の`response.sessionAttributes`フィールドを使用して途中までの情報をCEKに渡します。ユーザーの追加のリクエストを受け付けると、その情報は再びリクエストメッセージの`session.sessionAttributes`フィールドで渡されます。オブジェクトはキー（key）と値（value）のペアで構成され、Custom Extensionを実装する際、任意に定義できます。保存された値がない場合、空のオブジェクトが渡されます。   |  |
| `session.sessionId`                        | string  | セッションID                                                    |  |
| `session.user`                             | object  | 現在のユーザーの情報を持っているオブジェクト                             |  |
| `session.user.userId`                      | string  | 現在のユーザーのClova ID`context.System.user.userId`の値と異なることがあります。 |  |
| `session.user.accessToken`                 | string  | 特定のサービスのユーザーアカウントのアクセストークン。現在のユーザー関連付けられたユーザーアカウントのアクセストークンが渡されます。CEKは、外部サービスの認可サーバーから取得したユーザーアカウントのアクセストークンを渡します。詳細については、[ユーザーアカウントを連携する](/Develop/Guides/Link_User_Account.md)を参照してください。|条件付き |
| `version`                                  | string  | メッセージフォーマットのバージョン（CEKのバージョン）                          |  |

### Message example
{% raw %}
```json
//サンプル1：EventRequestタイプ
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
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b",
        "display": {
          "size": "l100",
          "orientation": "landscape",
          "dpi": 96,
          "contentLayer": {
            "width": 640,
            "height": 360
          }
        }
      }
    }
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
  }
}

//サンプル2：IntentRequestタイプ
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
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b",
        "display": {
          "size": "l100",
          "orientation": "landscape",
          "dpi": 96,
          "contentLayer": {
            "width": 640,
            "height": 360
          }
        }
      }
    }
  },
  "request": {
    "type": "IntentRequest",
    "intent": {
      "name": "OrderPizza",
      "slots": {
        "pizzaType": {
          "name": "pizzaType",
          "value": "ペパロニ"
        }
      }
    }
  }
}

//サンプル3：LaunchRequestタイプ
{
  "version": "0.1.0",
  "session": {
    "new": true,
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
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b",
        "display": {
          "size": "l100",
          "orientation": "landscape",
          "dpi": 96,
          "contentLayer": {
            "width": 640,
            "height": 360
          }
        }
      }
    }
  },
  "request": {
    "type": "LaunchRequest"
  }
}

//サンプル4：SessionEndedRequestタイプ
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
        "deviceId": "096e6b27-1717-33e9-b0a7-510a48658a9b",
        "display": {
          "size": "l100",
          "orientation": "landscape",
          "dpi": 96,
          "contentLayer": {
            "width": 640,
            "height": 360
          }
        }
      }
    }
  },
  "request": {
    "type": "SessionEndedRequest"
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [Custom Extensionでリクエストを処理する](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest)
* [AudioStreamInfoObject]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#AudioStreamInfoObject)

## リクエストタイプ {#CustomExtRequestType}
リクエストメッセージは、次の4つのタイプがあります。リクエストのタイプによって、リクエストメッセージの`request`オブジェクトのフィールドの構成が異なります。
* [`EventRequest`](#CustomExtEventRequest)
* [`IntentRequest`](#CustomExtIntentRequest)
* [`LaunchRequest`](#CustomExtLaunchRequest)
* [`SessionEndedRequest`](#CustomExtSessionEndedRequest)

### EventRequest {#CustomExtEventRequest}
{% if book.L10N.TargetCountryCode == "KR" %}
`EventRequest`タイプは、クライアントの状態の変化や、それに伴うリクエストをExtensionに渡すために使用されるリクエストタイプです。CEKは、`EventRequest`リクエストタイプを使用して以下の項目を処理します。
* クライアントの[オーディオ再生状態をExtensionにレポートする](/Develop/Guides/Build_Custom_Extension.md#CollectPlaybackStatusAndProgress)
* [オーディオ再生に関連する付加情報をExtensionにリクエストする](/Develop/Guides/Build_Custom_Extension.md#ProvidingMetaDataForDisplay)
* クライアントの[合成音声（TTS）の再生状態をExtensionにレポートする](/Develop/Guides/Monitor_TTS_Playback_Status.md)

Extensionの開発者は、オーディオや合成音声の再生状態のレポート、または付加情報のリクエストに応じて適切に処理する必要があります。現在`EventRequest`リクエストタイプで上記の項目を処理するとき、以下の[CIC API]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }})の[イベント]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/CIC_API.{{ book.DocMeta.FileExtensionForExternalLink }}#Event)を使用します。

* [`AudioPlayer.PlayFinished`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayFinished)
* [`AudioPlayer.PlayPaused`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayPaused)
* [`AudioPlayer.PlayResumed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayResumed)
* [`AudioPlayer.PlayStarted`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayStarted)
* [`AudioPlayer.PlayStopped`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#PlayStopped)
* [`AudioPlayer.ProgressReportDelayPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}#StreamRequested)
* [`SpeechSynthesizer.SpeechFinished`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#SpeechStopped)
* [`TemplateRuntime.RequestPlayerInfo`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/TemplateRuntime.{{ book.DocMeta.FileExtensionForExternalLink }}#RequestPlayerInfo)
{% elif book.L10N.TargetCountryCode == "JP" %}
`EventRequest`タイプは、クライアントの状態の変化や、それに伴うリクエストをExtensionに渡すために使用されるリクエストタイプです。CEKは、`EventRequest`リクエストタイプを使用して以下の項目を処理します。
* ユーザーが特定のスキルを有効または無効にした結果をレポートする
* クライアントの[オーディオ再生状態をExtensionにレポートする](/Develop/Guides/Build_Custom_Extension.md#CollectPlaybackStatusAndProgress)
* [オーディオ再生に関連する付加情報をExtensionにリクエストする](/Develop/Guides/Build_Custom_Extension.md#ProvidingMetaDataForDisplay)
* クライアントの[合成音声（TTS）の再生状態をExtensionにレポートする](/Develop/Guides/Monitor_TTS_Playback_Status.md)

Extensionの開発者は、スキルの有効化/無効化、オーディオや音声の再生状態のレポート、または付加情報のリクエストに応じて、適切に処理する必要があります。

`EventRequest`リクエストタイプでオーディオ再生状態をレポートしたり、付加情報のリクエストをExtensionに送信するとき、以下の[CIC API](#CICAPIforAudioPlayback)を使用します。

* [`AudioPlayer.PlayFinished`](#PlayFinished)
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)
{% endif %}

`EventRequest`タイプのメッセージの`request`オブジェクトのフィールドは、次のように構成されます。

{% raw %}
```json
{
  "type": "EventRequest",
  "requestId": {{string}},
  "timestamp": {{string}},
  "event": {
    "namespace": {{string}},
    "name": {{string}},
    "payload": {{object}}
  }
}
```
{% endraw %}

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `event`           | object  | クライアントがClovaに渡した情報が保存されているオブジェクト                                       |    |
| `event.name`      | string  | {{ "クライアントからClovaに送信したイベントの名前" if book.L10N.TargetCountryCode == "KR" else "クライアントからClovaに送信したイベントの名前、または、スキルの有効化/無効化を表す名前。スキルの有効化/無効化を表す名前は、`SkillEnabled`/`SkillDisabled`になります。スキルの有効化/無効化に関するリクエストを受信した場合、[リクエストメッセージ](#CustomExtRequestMessage)の`context.System.application.applicationId`フィールドと`context.System.user.userId`フィールドを使用して、ユーザーの情報を取得したり、ユーザーの情報を廃棄したりするように実装することができます。"}} |    |
| `event.namespace` | string  | {{ "クライアントからClovaに送信したイベントの名前空間" if book.L10N.TargetCountryCode == "KR" else "クライアントからClovaに送信したイベントの名前空間、または、スキルの有効化/無効化を表す名前空間。スキルの有効化/無効化を表す名前空間は、`ClovaSkill`に固定されます。"}}   |   |
| `event.payload`   | object  | {{ "クライアントからClovaに送信したイベントの`payload`、または`payload`の一部の情報。一部のイベントの`EventRequest`リクエストタイプは、`payload`が空のオブジェクトの場合があります。"if book.L10N.TargetCountryCode == "KR" else "クライアントからClovaに送信したイベントの`payload`、または`payload`の一部の情報。一部のイベントや、スキルの有効化/無効化を表すための`EventRequest`リクエストタイプは、`payload`が空のオブジェクトの場合があります。"}}  |   |
| `requestId`       | string  | クライアントがClovaに情報を渡すときに作成されたダイアログID（`event.header.dialogRequestId`）    |    |
| `timestamp`       | string  | クライアントがClovaに情報を渡した日時（`YYYY-MM-DDThh:mm:ssZ`<a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601</a>形式）<div class="tip"><p><strong>ヒント</strong></p><p>CEKは<code>EventRequest</code>タイプのリクエストの順序を保証しません。クライアントからのリクエストの順序は、このフィールドの値から把握する必要があります。</p></div>                    |   |
| `type`            | string  | リクエストメッセージのタイプ。`"EventRequest"`の値に固定されます。         |  |

以下は、`EventRequest`タイプのメッセージの`request`オブジェクトフィールドのサンプルです。

{% if book.L10N.TargetCountryCode == "KR" %}
```json
// サンプル1. 音楽のメタデータをリクエストしたとき
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

// サンプル2. 音楽再生を停止したとき
"request": {
  "type": "EventRequest",
  "requestId": "e5464288-50ff-4e99-928d-4a301e083d41",
  "timestamp": "2017-09-05T05:41:21Z",
  "event": {
    "namespace": "AudioPlayer",
    "name": "PlayStopped",
    "payload": {}
  }
}
```
{% elif book.L10N.TargetCountryCode == "JP" %}
```json
// サンプル1. ユーザーがスキルを有効にしたとき
"request": {
  "type": "EventRequest",
  "requestId": "f09874hiudf-sdf-4wku-flksdjfo4hjsdf",
  "timestamp": "2018-06-11T09:19:23Z",
  "event" : {
    "namespace":"ClovaSkill",
    "name":"SkillEnabled",
    "payload": null
  }
}

// サンプル2. ユーザーがスキルを無効にしたとき
"request": {
  "type": "EventRequest",
  "requestId": "f09874hiudf-sdf-4wku-flksdjfo4hjsdf",
  "timestamp": "2018-06-19T11:37:21Z",
  "event" : {
    "namespace":"ClovaSkill",
    "name":"SkillDisabled",
    "payload": null
  }
}

// サンプル3. 音楽のメタデータをリクエストしたとき
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

// サンプル4. 音楽再生を停止したとき
"request": {
  "type": "EventRequest",
  "requestId": "e5464288-50ff-4e99-928d-4a301e083d41",
  "timestamp": "2017-09-05T05:41:21Z",
  "event": {
    "namespace": "AudioPlayer",
    "name": "PlayStopped",
    "payload": {}
  }
}
```
{% endif %}

### IntentRequest {#CustomExtIntentRequest}
`IntentRequest`は、解析されたユーザーのリクエストを渡し、その内容を処理するように要求するリクエストタイプです。Extensionの開発者はサービスを開発する際、ユーザーのリクエストをどう受け付けるかについて[対話モデルを定義](/Design/Design_Custom_Extension.md#DefineInteractionModel)する必要があります。対話モデルは、[Clova Developer Center](/DevConsole/ClovaDevConsole_Overview.md)で登録できます。その際、区別されるユーザーのリクエストをインテントという情報形式で定義します。解析されたユーザーの発話情報はインテントに変換され、`intent`フィールドでExtensionに渡されます。

`IntentRequest`タイプのメッセージの`request`オブジェクトのフィールドは、次のように構成されます。

{% raw %}
```json
{
  "type": "IntentRequest",
  "intent": {
    "name": {{string}},
    "slots": {{object}}
  }
}
```
{% endraw %}

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `intent`        | object  | ユーザーのリクエストを解析した情報が保存されたオブジェクト[インテント](/Design/Design_Custom_Extension.md#Intent)                          |  |
| `intent.name`   | string  | インテント名対話モデルに定義した[インテント](/Design/Design_Custom_Extension.md#Intent)をこのフィールドで識別できます。  |  |
| `intent.slots`  | object  | Extensionがインテントを処理する際に要求される情報（スロット）が保存されたオブジェクト。このフィールドは、`intent.name`フィールドに入力された[インテント](/Design/Design_Custom_Extension.md#Intent)によって構成が異なることがあります。 |  |
| `type`          | string  | リクエストメッセージのタイプ。`"IntentRequest"`の値に固定されます。                                                                     |  |

以下は、`IntentRequest`タイプのメッセージの`request`オブジェクトフィールドのサンプルです。

```json
"request": {
  "type": "IntentRequest",
  "intent": {
    "name": "OrderPizza",
    "slots": {
      "pizzaType": {
        "name": "pizzaType",
        "value": "ペパロニ"
      }
    }
  }
}
```

### LaunchRequest {#CustomExtLaunchRequest}
`LaunchRequest`は、ユーザーが特定のExtensionの使用を開始したことを示すリクエストタイプです。例えば、ユーザーが「サイコロ遊びを起動して」と言ったときのように、特定のスキルを使用すると宣言した状況です。ユーザーがスキルの使用をやめると宣言するまで、そのExtensionから[`IntentRequest`](#CustomExtIntentRequest)タイプのメッセージを受信します。

`LaunchRequest`タイプのメッセージの`request`オブジェクトのフィールドは、次のように構成されます。

{% raw %}
```json
{
  "type": "LaunchRequest"
}
```
{% endraw %}

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `type`          | string  | リクエストメッセージのタイプ。`"LaunchRequest"`の値に固定されます。 |  |

### SessionEndedRequest {#CustomExtSessionEndedRequest}
`SessionEndedRequest`タイプは、ユーザーの特定のスキルの使用が終了したことを示すリクエストです。次の状況でこのメッセージを受信します。
* ユーザーがスキルの終了をリクエストした場合
* 特定の時間内にユーザーからの入力がない場合（Timeout）
* エラーが発生した場合

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p><a href="#CustomExtResponseMessage">レスポンスメッセージ</a>の<code>shouldEndSession</code>フィールドを使用して、Extensionから先に終了を宣言した場合には、このメッセージを受信しません。</p>
</div>

`SessionEndedRequest`タイプのメッセージの`request`オブジェクトのフィールドは、次のように構成されます。

{% raw %}
```json
{
  "type": "SessionEndedRequest"
}
```
{% endraw %}

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `type`          | string  | リクエストメッセージのタイプ。`"SessionEndedRequest"`の値に固定されます。 |  |

## レスポンスメッセージ {#CustomExtResponseMessage}
Extensionは、リクエストメッセージを処理して、レスポンスメッセージを渡す必要があります（HTTPレスポンス）。ここでは、レスポンスメッセージの構造と各フィールドについて説明します。

### Message structure
{% raw %}
```json
{
  "response": {
    "card": {{object}},
    "directives": [
      {
        "header": {
          "messageId": {{string}},
          "name": {{string}},
          "namespace": {{string}}
        },
        "payload": {{object}}
      }
    ],
    "outputSpeech": {
      "type": {{string}},
      "values": {{SpeechInfoObject|SpeechInfoObject array}},
      "brief": {{SpeechInfoObject}},
      "verbose": {
        "type": {{string}},
        "values": {{SpeechInfoObject|SpeechInfoObject array}},
      }
    },
    "reprompt": {
      "outputSpeech": {
        "type": {{string}},
        "values": {{SpeechInfoObject|SpeechInfoObject array}},
        "brief": {{SpeechInfoObject}},
        "verbose": {
          "type": {{string}},
          "values": {{SpeechInfoObject|SpeechInfoObject array}},
        }
      }
    },
    "shouldEndSession": {{boolean}},
  },
  "sessionAttributes": {{object}},
  "version": {{string}}
}
```
{% endraw %}

### Message fields
| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `response`                               | object       | Extensionのレスポンス情報を含むオブジェクト                            |  |
| `response.card`                          | object       | [コンテンツテンプレート]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Content_Templates.{{ book.DocMeta.FileExtensionForExternalLink }}) 形式のデータで、クライアントの画面に表示するコンテンツをこのフィールドで渡すことができます。このフィールドにデータがある場合、CICはクライアントに[Clova.RenderTemplate]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/Clova.{{ book.DocMeta.FileExtensionForExternalLink }}#RenderTemplate)ディレクティブを送信します。空のオブジェクトの場合、CICはクライアントに[Clova.RenderText]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/Clova.{{ book.DocMeta.FileExtensionForExternalLink }}#RenderText)ディレクティブを送信し、`response.outputSpeech.values`フィールドの値を表示するようにします。        |  |
| `response.directives[]`                  | object array | ExtensionがCEKに渡すディレクティブです。`response.directives`フィールドは、主にオーディオコンテンツを提供するために使用されます。次の {%- if book.L10N.TargetCountryCode == "KR" -%}[CIC API](https://pages.oss.navercorp.com/JTF-P6/Clova_Client_Developer_Guide/ko-KR/Develop/References/CIC_API{{ book.DocMeta.FileExtensionForExternalLink }}){%- elif book.L10N.TargetCountryCode == "JP" -%}[CIC API](#CICAPIforAudioPlayback){%- endif -%} ディレクティブがサポートされています。<ul><li><code>AudioPlayer.Play</code></li><li><code>AudioPlayer.StreamDeliver</code></li><li><code>PlaybackController.Pause</code></li><li><code>PlaybackController.Resume</code></li><li><code>PlaybackController.Stop</code></li><li><code>TemplateRuntime.RenderPlayerInfo</code></li></ul><div class="note"><p><strong>メモ</strong></p><p>CIC APIディレクティブ</p>を作成するとき、「Optional」項目が<strong>空白</strong>のフィールドは必須なので、必ず作成してください。</p></div> |  |
| `response.directives[].header`           | object       | ディレクティブのヘッダー                                          |  |
| `response.directives[].header.messageId` | string       | メッセージID（UUID）。メッセージを区別するための識別子です。   |  |
| `response.directives[].header.name`      | string       | ディレクティブのAPI名                                      |  |
| `response.directives[].header.namespace` | string       | ディレクティブのAPI名前空間                                |  |
| `response.directives[].payload`          | object       | ディレクティブに関する情報を持つオブジェクト。ディレクティブに応じて、payloadオブジェクトの構成とフィールド値を変更できます。         |  |
| `response.outputSpeech`                  | object       | 音声に合成する情報を含んでいるオブジェクト。合成された音声はCICを介してクライアントに渡されます。              |  |
| `response.outputSpeech.brief`            | [SpeechInfoObject](#CustomExtSpeechInfoObject) | 出力する要約音声情報                    | 任意 |
| `response.outputSpeech.type`             | string       | 出力する音声情報のタイプ<ul><li><code>"SimpleSpeech"</code>：単文タイプの音声情報です。最も基本となるタイプで、この値を指定した場合、<code>response.outputSpeech.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクトを持っている必要があります。</li><li><code>"SpeechList"</code>：複文タイプの音声情報です。複数の文章を出力する際に使用されます。この値を指定した場合、<code>response.outputSpeech.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクト配列を持っている必要があります。</li><li><code>"SpeechSet"</code>：複合タイプの音声情報です。画面を持たないクライアントデバイスに、要約音声情報と詳細音声情報を渡す際に使用されます。この値を指定した場合、<code>response.outputSpeech.values</code>フィールドの代わりに<code>response.outputSpeech.brief</code>と<code>response.outputSpeech.verbose</code>フィールドを持っている必要があります。</li></ul> |  |
| `response.outputSpeech.values[]`           | [SpeechInfoObject](#CustomExtSpeechInfoObject) or [SpeechInfoObject](#CustomExtSpeechInfoObject) array | クライアントデバイスで出力する音声情報を持っているオブジェクトまたはオブジェクト配列 | 任意 |
| `response.outputSpeech.verbose`          | object       | 画面を持たないクライアントデバイスに渡す際に使用されます。詳細音声情報を含んでいます。 | 選択 |
| `response.outputSpeech.verbose.type`     | string       | 出力する音声情報のタイプ単文と複文タイプの音声情報のみ入力できます。<ul><li><code>"SimpleSpeech"</code>：単文タイプの音声情報です。最も基本的な音声情報を渡す際に使用されます。この値を指定した場合、<code>response.outputSpeech.verbose.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクトを持っている必要があります。</li><li><code>"SpeechList"</code>：複文タイプの音声情報です。複数の文章を出力する際に使用されます。この値を指定した場合、<code>response.outputSpeech.verbose.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクト配列を持っている必要があります。</li></ul> |  |
| `response.outputSpeech.verbose.values[]`           | [SpeechInfoObject](#CustomExtSpeechInfoObject) or [SpeechInfoObject](#CustomExtSpeechInfoObject) array | クライアントデバイスで出力する詳細音声情報を持っているオブジェクトまたはオブジェクト配列 |  |
| `response.reprompt`                               | obejct       | ユーザーの追加の発話を促す音声情報を含んでいるオブジェクト。`response.reprompt`フィールドを使用すると、ユーザーにマルチターン対話を続けるか尋ねたり、または必須情報を話すように促すことができます。通常、マルチターン対話を行う際、ユーザーが追加の発話をしないと、入力待ち時間が過ぎ、マルチターン対話が自動的に終了します。ただし、`response.reprompt`フィールドを使用すると、Clovaは、入力待ち時間が過ぎた後、クライアントが`response.reprompt`フィールドに作成した音声を出力し、再度ユーザーの追加の発話を促すように[`SpeechSynthesizer.Speak`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechSynthesizer.{{ book.DocMeta.FileExtensionForExternalLink }}#Speak)ディレクティブと[`SpeechRecognizer.ExpectSpeech`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/SpeechRecognizer.{{ book.DocMeta.FileExtensionForExternalLink }}#ExpectSpeech)ディレクティブをクライアントに渡します。<div class="tip"><p><strong>ヒント</strong></p><p><code>response.reprompt</code>フィールドは、<code>response.shouldEndSession</code>フィールド値を<code>false</code>に入力した場合、有効です。主に単文タイプの音声情報（<code>"SimpleSpeech"</code>）を渡すことをお勧めします。<code>response.reprompt</code>フィールドを使用すると、入力待ち時間を最大1回延長できます。</p></div> | 選択 |
| `response.reprompt.outputSpeech`                  | object       | 音声に合成する情報を含んでいるオブジェクト。合成された音声はCICを介してクライアントに渡されます。              |  |
| `response.reprompt.outputSpeech.brief`            | [SpeechInfoObject](#CustomExtSpeechInfoObject) | 出力する要約音声情報                    | 選択 |
| `response.reprompt.outputSpeech.type`             | string       | 出力する音声情報のタイプ<ul><li>"SimpleSpeech"：単文タイプの音声情報です。最も基本となるタイプで、この値を指定した場合、<code>response.outputSpeech.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクトを持っている必要があります。</li><li><code>"SpeechList"</code>：複文タイプの音声情報です。複数の文章を出力する際に使用されます。この値を指定した場合、<code>response.outputSpeech.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクト配列を持っている必要があります。</li><li><code>"SpeechSet"</code>：複合タイプの音声情報です。画面を持たないクライアントデバイスに、要約音声情報と詳細音声情報を渡す際に使用されます。この値を指定した場合、<code>response.outputSpeech.values</code>フィールドの代わりに<code>response.outputSpeech.brief</code>と<code>response.outputSpeech.verbose</code>フィールドを持っている必要があります。</li></ul> |  |
| `response.reprompt.outputSpeech.values[]`           | [SpeechInfoObject](#CustomExtSpeechInfoObject) or [SpeechInfoObject](#CustomExtSpeechInfoObject) array | クライアントデバイスで出力する音声情報を持っているオブジェクトまたはオブジェクト配列 | 選択 |
| `response.reprompt.outputSpeech.verbose`          | object       | 画面を持たないクライアントデバイスに渡す際に使用されます。詳細音声情報を含んでいます。 | 選択 |
| `response.reprompt.outputSpeech.verbose.type`     | string       | 出力する音声情報のタイプ単文と複文タイプの音声情報のみ入力できます。<ul><li><code>"SimpleSpeech"</code>：単文タイプの音声情報です。最も基本的な音声情報を渡す際に使用されます。この値を指定した場合、<code>response.outputSpeech.verbose.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクトを持っている必要があります。</li><li><code>"SpeechList"</code>：複文タイプの音声情報です。複数の文章を出力する際に使用されます。この値を指定した場合、<code>response.outputSpeech.verbose.values</code>フィールドが<a href="#CustomExtSpeechInfoObject"><code>SpeechInfoObject</code></a>オブジェクト配列を持っている必要があります。</li></ul> |  |
| `response.reprompt.outputSpeech.verbose.values[]`           | [SpeechInfoObject](#CustomExtSpeechInfoObject) or [SpeechInfoObject](#CustomExtSpeechInfoObject) array | クライアントデバイスで出力する詳細音声情報を持っているオブジェクトまたはオブジェクト配列 |  |
| `response.shouldEndSession`              | boolean      | セッション終了のフラグクライアントに特定のExtensionの使用が終了したことを示すフィールドです。[`SessionEndedRequest`](#CustomExtSessionEndedRequest)タイプのリクエストメッセージを受け取る前に、Extensionから先に使用終了を示す際に使用されます。<ul><li>true：使用を終了する</li><li>false：引き続き使用する。ユーザーとマルチターン対話を行います。</li></ul> |  |
| `sessionAttributes`                      | object       | ユーザーとのマルチターン対話に必要な情報を保存するために使用されるオブジェクト。Custom Extensionは、`sessionAttributes`フィールドを使用して途中までの情報をCEKに渡します。ユーザーの追加のリクエストを受け付けると、その情報は再び[リクエストメッセージ](#CustomExtRequestMessage)の`session.sessionAttributes`フィールドで渡されます。`sessionAttributes`オブジェクトは、キー（key）と値（value）のペアで構成され、Custom Extensionを実装する際に任意で定義できます。保存する値がない場合、空のオブジェクトを入力します。 |  |
| `version`                                | string       | メッセージフォーマットのバージョン（CEKのバージョン）                        |  |

<div class="note">
  <p><strong>メモ</strong></p>
  <p><code>response.directives</code>フィールドでExtensionに任意のディレクティブを渡すときに使用するには、事前に協議が必要です。提携担当者と協議してください。</p>
</div>

### SpeechInfoObject {#CustomExtSpeechInfoObject}
SpeechInfoObjectオブジェクトはレスポンスメッセージの`response.outputSpeech`で再使用されるオブジェクトです。ユーザーに出力する音声情報の最も小さな単位である単文レベルの発話情報です。このオブジェクトは、次のフィールドを持ちます。

| フィールド名        | データ型         | 説明                                                                | 必須/任意 |
|----------------|--------------|--------------------------------------------------------------------|:-----:|
| `contentType`    | string       | HLS形式のストリームを提供する場合、`"application/vnd.apple.mpegurl"`を入力します。  | 任意   |
| `lang`           | string       | 音声を合成する際に使用する言語のコード。現在、次の値を持ちます。<ul><li><code>"en"</code>：英語</li><li><code>"ja"</code>：日本語</li><li><code>"ko"</code>：韓国語</li><li><code>""</code>：<code>type</code>フィールドの値が<code>"URL"</code>の場合、このフィールドは空の文字列（empty string）を持ちます。</li></ul>         |  |
| `token`          | string       | 提供する音声のトークン。このフィールドに値を入力すると、クライアントから該当音声を再生した結果や再生状態のレポートを受信します。音声再生状態の進行状況のレポートは、[`EventRequest`リクエストタイプ](/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest)のメッセージで送信されます。UUID形式（version 4）で作成される必要があります。<div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは2048バイトです。</p></div> | 任意  |
| `type`           | string       | 提供する音声のタイプ。このフィールドの値によって、`value`フィールドの値の形式が異なります。現在、次の値を持ちます。<ul><li><code>"PlainText"</code>：テキスト</li><li><code>"URL"</code>：音声および音楽を再生できるファイルのURI</li></ul>            |  |
| `value`          | string       | 音声合成する内容または音声ファイルのURI。<div class="tip"><p><strong>ヒント</strong></p><p>Clovaでサポートされる音声ファイルフォーマットについての詳細は、<a href="/Design/Supported_Audio_Format.md">プラットフォームでサポートされる音声ファイルフォーマット</a>を参照してください。</p></div><div class="note"><p><strong>メモ</strong></p><p><code>type</code>フィールドが<code>URL</code>の場合、このフィールドの最大長さは2048バイトです。</p></div>     |  |

### Message example
{% raw %}
```json
//サンプル1：単文タイプ（SimpleSpeech）の音声情報を返す-テキスト
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "en",
          "value": "Hi, nice to meet you"
      }
    },
    "card": {},
    "directives": [],
    "shouldEndSession": false
  }
}

//サンプル2：複文タイプ（SpeechList）の音声情報を返す-テキスト、"URL"を使用
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SpeechList",
      "values": [
        {
          "type": "PlainText",
          "lang": "ja",
          "value": "歌を歌ってみます。"
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

//サンプル3：複文タイプ（SpeechList）の音声情報を返す場合-テキスト、"URL"形式を使用（HLSストリームを使用）
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SpeechList",
      "values": [
        {
          "type": "PlainText",
          "lang": "ja",
          "value": "歌を歌ってみます。"
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

//サンプル4：複合タイプ（SpeechSet）の音声情報を返す-要約・詳細音声情報
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {
      "type": "SpeechSet",
      "brief": {
        "type": "PlainText",
        "lang": "ja",
        "value": "天気予報です。"
      },
      "verbose": {
        "type": "SpeechList",
        "values": [
          {
              "type": "PlainText",
              "lang": "ja",
              "value": "週末まで全国に梅雨…猛暑和らぐ。"
          },
          {
              "type": "PlainText",
              "lang": "ja",
              "value": "明日全国的に梅雨…ところによって局地的に激しい雨に注意。"
          }
          ...
        ]
      }
    },
    "card": {},
    "directives": [],
    "shouldEndSession": true
  }
}

//サンプル5：マルチターン対話で、対話の中間情報を保存する-sessionAttributesを使用
{
  "version": "0.1.0",
  "sessionAttributes": {
    "RequestedIntent": "OrderPizza",
    "pizzaType": "ペパロニピザ"
  },
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "ja",
          "value": "何枚注文しますか?"
      }
    },
    "card": {},
    "directives": [],
    "shouldEndSession": false
  }
}

//サンプル6：マルチターン対話でユーザーの追加の発話を促す-repromptを使用
{
  "version": "0.1.0",
  "sessionAttributes": {
    "RequestedIntent": "OrderPizza",
    "pizzaType": "ペパロニピザ"
  },
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "ja",
          "value": "何枚注文しますか?"
      }
    },
    "card": {},
    "reprompt" : {
      "outputSpeech" : {
        "type" : "SimpleSpeech",
        "values" : {
          "type" : "PlainText",
          "lang" : "ja",
          "value" : "お言葉がなければ、注文をキャンセルしてよろしいですか?"
        }
      }
    },
    "shouldEndSession": false
  }
}

//サンプル7：クライアントにオーディオコンテンツを再生するように指示するレスポンス（response.directives[]フィールドを使用）
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
            },
          },
          "playBehavior": "REPLACE_ALL"
        }
      }
    ],
    "outputSpeech": {},
    "shouldEndSession": true
  }
}

//サンプル8：音声トークンを入力する場合-再生の進行状況を確認するためのもの
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
          "value": "Hi, nice to meet you"
      }
    },
    "card": {},
    "directives": [],
    "shouldEndSession": false
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [Custom Extensionでレスポンスを返す](/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse)
* [Content template]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Content_Templates.{{ book.DocMeta.FileExtensionForExternalLink }})

{% if book.L10N.TargetCountryCode == "JP" %}

## オーディオコンテンツ再生関連のCIC API {#CICAPIforAudioPlayback}

<!-- Start of the shared content: CICAPIforAudioPlayback -->

CIC APIは、ユーザーのクライアントデバイスがClovaと通信を行うときにやり取りするメッセージの規格です。ここで説明されているCIC APIとは、Custom Extensionの[オーディオコンテンツの提供](/Develop/Guides/Provide_Audio_Content.md)に関連して、CEKでサポートしているCIC APIのことです。CEKは、クライアントがClovaに渡したイベントを、[`EventRequest`](#CustomExtEventRequest)タイプのメッセージの`event`フィールドに設定して送信します。また、Custom Extensionは、ユーザーにオーディオコンテンツを提供するために、オーディオコンテンツの再生コントロールに関連するディレクティブを、[レスポンスメッセージ](#CustomExtResponseMessage)の`response.directives[]`フィールドに設定して送信する必要があります。

従って、Custom Extensionがオーディオコンテンツを提供する場合、以下のCIC APIを理解しておく必要があります。

| 名前空間   | メッセージ         | タイプ  | 説明                                   |
|-------------|------------------|---------|---------------------------------------------|
| AudioPlayer | [`Play`](#Play)                       | ディレクティブ | クライアントに対して、特定のオーディオストリームを再生するか、または再生キューに追加するように指示します。                         |
| AudioPlayer | [`PlayFinished`](#PlayFinished)       | イベント     | クライアントがオーディオストリームの再生を終了するとき、そのオーディオストリームの情報をCICにレポートするために使用します。     |
| AudioPlayer | [`PlayPaused`](#PlayPaused)           | イベント     | クライアントがオーディオストリームの再生を一時停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。 |
| AudioPlayer | [`PlayResumed`](#PlayResumed)         | イベント     | クライアントがオーディオストリームの再生を再開するとき、そのオーディオストリームの情報をCICにレポートするために使用します。         |
| AudioPlayer | [`PlayStarted`](#PlayStarted)         | イベント     | クライアントがオーディオストリームの再生を開始するとき、そのオーディオストリームの情報をCICにレポートするために使用します。    |
| AudioPlayer | [`PlayStopped`](#PlayStopped)         | イベント     | クライアントがオーディオストリームの再生を停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。    |
| AudioPlayer | [`ProgressReportDelayPassed`](#ProgressReportPositionPassed) | イベント | オーディオストリームの再生が開始してから、指定された遅延期間が経過したタイミングの再生状態をCICにレポートするために使用します。 |
| AudioPlayer | [`ProgressReportIntervalPassed`](#ProgressReportPositionPassed)| イベント | オーディオストリームの再生が開始してから、指定された間隔ごとの再生状態をCICにレポートするために使用します。 |
| AudioPlayer | [`ProgressReportPositionPassed`](#ProgressReportPositionPassed) | イベント | オーディオストリームの再生が開始してから、指定されたタイミングに、そのときの再生状態をCICにレポートするために使用します。 |
| AudioPlayer | [`StreamDeliver`](#StreamDeliver)     | ディレクティブ | [`AudioPlayer.StreamRequested`](#StreamRequested)イベントに対する応答です。実際に再生できるオーディオストリームの情報を受信するために使用します。 |
| AudioPlayer | [`StreamRequested`](#StreamRequested) | イベント     | オーディオストリームを再生するために、ストリーミングのURIなど、追加の情報をCICにリクエストするイベントです。               |
| PlaybackController | [`Pause`](#Pause)              | ディレクティブ | クライアントに、再生中のオーディオストリームを一時停止するように指示します。        |
| PlaybackController | [`Resume`](#Resume)            | ディレクティブ | クライアントに、オーディオストリームの再生を再開するように指示します。                |
| PlaybackController | [`Stop`](#Stop)                | ディレクティブ | クライアントに、オーディオストリームの再生を停止するように指示します。                |
| SpeechSynthesizer | [`SpeechFinished`](#SpeechFinished) | イベント | クライアントから、合成音声の再生を完了したことをレポートします。  |
| SpeechSynthesizer | [`SpeechStarted`](#SpeechStarted)   | イベント | クライアントから、合成音声の再生を開始したことをレポートします。  |
| SpeechSynthesizer | [`SpeechStopped`](#SpeechStopped)   | イベント | クライアントから、合成音声の再生を停止したことをレポートします。  |
| TemplateRuntime | [`RenderPlayerInfo`](#RenderPlayerInfo)        | ディレクティブ | CICから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをクライアントに送信し、表示するように指示します。 |
| TemplateRuntime | [`RequestPlayerInfo`](#RequestPlayerInfo)      | イベント     | クライアントから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをCICにリクエストします。 |

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.Play -->

### AudioPlayer.Playディレクティブ {#Play}
クライアントに対して、特定のオーディオストリームを再生するか、または再生キューに追加するように指示します。

#### Payload fields
| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `audioItem`               | object | 再生するオーディオストリームのメタデータと、再生に必要なオーディオストリームの情報を持つオブジェクト                     |  |
| `audioItem.artImageUrl`   | string | オーディオコンテンツに関連する画像（アルバムの画像）のURI                                                  | 条件付き  |
| `audioItem.audioItemId`   | string | オーディオストリームを区別するID。クライアントはこの値に基づいて、重複するPlayディレクティブを削除できます。 |  |
| `audioItem.headerText`    | string | 主に、現在の再生リストのタイトルを表すテキストフィールド                                                | 条件付き  |
| `audioItem.stream`        | [AudioStreamInfoObject](#AudioStreamInfoObject) | 再生に必要なオーディオストリームの情報を持つオブジェクト        |  |
| `audioItem.titleSubText1` | string | 主にアーティスト名を表すテキストフィールド                                                          |  |
| `audioItem.titleSubText2` | string | 主にアルバム名を表すサブテキストフィールド                                                      | 条件付き |
| `audioItem.titleText`     | string | 現在のオーディオコンテンツのタイトルを表すテキストフィールド                                                         |   |
| `playBehavior`            | string | ディレクティブに含まれたオーディオストリームを、クライアントでいつ再生するかを指定するフィールド<ul><li><code>"REPLACE_ALL"</code>：再生キューをすべてクリアして、送信されたオーディオストリームをすぐに再生します。</li><li><code>"ENQUEUE"</code>：再生キューに、送信されたオーディオストリームを追加します。</li></ul> |  |
| `source`                  | object | オーディオストリーミングサービスの提供元                                                    |  |
| `source.logoUrl`          | string | オーディオストリーミングサービスのロゴ画像のURI。このフィールドまたはフィールドの値がなかったり、ロゴ画像を表示できない場合、`source.name`フィールド内のオーディオストリーミングサービスの名前を表示する必要があります。  | 条件付き |
| `source.name`             | string | オーディオストリーミングサービスの名前                                                        |  |

#### 備考
ストリーミングサービスの課金などの理由により、実際のストリーミング情報、つまりストリーミングのURIなどの情報を、再生する直前に取得する必要がある場合があります。`audioItem.stream.urlPlayable`フィールドの値によって、次のように区分されます。
* `urlPlayable`フィールドの値が`true`の場合、`audioItem.stream.url`フィールドに含まれたURIでオーディオストリームをすぐに再生できます。
* `urlPlayable`フィールドの値が`false`の場合、`audioItem.stream.url`フィールドに含まれたURIではオーディオストリームをすぐに再生できず、[`AudioPlayer.StreamRequested`](#StreamRequested)イベントでオーディオストリームの情報を追加でリクエストする必要があります。

#### Message example
{% raw %}

```json
// すぐに再生できるオーディオストリームのURIが含まれたサンプル
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
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
}

// すぐに再生できないオーディオストリームのURIが含まれたサンプル
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
        "album": {
          "albumId": "2000240",
          "genres": [
            "Classic"
          ],
          "title": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces"
        },
        ...
        "stream": {
          "beginAtInMilliseconds": 0,
          "durationInMilliseconds": 60000,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "clova:TR-NM-17716562",
          "urlPlayable": false
        },
        "title": "Symphony No.4 In A Op.90 'Italian' - III.Con Moto Moderato",
        "type": "SampleMusicProvider"
      },
      "source": {
        "name": "Sample Music Provider",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayFinished -->

### AudioPlayer.PlayFinishedイベント {#PlayFinished}
クライアントがオーディオストリームの再生を終了するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン    |  |
| `offsetInMilliseconds` | number | クライアントが再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayFinished",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 183000
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayPaused -->

### AudioPlayer.PlayPausedイベント {#PlayPaused}
クライアントがオーディオストリームの再生を一時停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | クライアントが再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayPaused",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayResumed`](#PlayResumed)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayResumed -->

### AudioPlayer.PlayResumedイベント {#PlayResumed}

クライアントがオーディオストリームの再生を再開するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | クライアントが再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayResumed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayPaused`](#PlayPaused)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStarted -->

### AudioPlayer.PlayStartedイベント {#PlayStarted}
クライアントがオーディオストリームの再生を開始するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | クライアントが再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 0
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStopped`](#PlayStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStopped -->

### AudioPlayer.PlayStoppedイベント {#PlayStopped}
クライアントがオーディオストリームの再生を停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "PlayStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStarted`](#PlayStarted)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportDelayPassed -->

### AudioPlayer.ProgressReportDelayPassedイベント {#ProgressReportDelayPassed}
オーディオストリームの再生が開始してから、指定された遅延期間が経過したタイミングの再生状態をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ProgressReportDelayPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 60000
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportIntervalPassed -->

### AudioPlayer.ProgressReportIntervalPassedイベント {#ProgressReportIntervalPassed}
オーディオストリームの再生が開始してから、指定された間隔ごとの再生状態をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ProgressReportIntervalPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 120000
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportPositionPassed -->

### AudioPlayer.ProgressReportPositionPassedイベント {#ProgressReportPositionPassed}
オーディオストリームの再生が開始してから、指定されたタイミングに、そのときの再生状態をCICにレポートするために使用します。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`                | string | オーディオストリームのトークン |  |
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ProgressReportPositionPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 150000
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.StreamDeliver -->

### AudioPlayer.StreamDeliverディレクティブ {#StreamDeliver}
[`AudioPlayer.StreamRequested`](#StreamRequested)イベントに対する応答です。実際に再生できるオーディオストリームの情報を受信するために使用します。クライアントがコンテンツを再生できるように、オーディオストリームの情報には、ストリーミングURIが必ず含まれます。

#### Payload fields
| フィールド名 | データ型 | フィールドの説明 | 任意 |
|---------|------|--------|:---------:|
| `audioItemId` | string | オーディオストリームの情報を区別するための値。クライアントはこの値に基づいて、重複するPlayディレクティブを削除できます。 |  |
| `audioStream` | [AudioStreamInfoObject](#AudioStreamInfoObject) | 再生に必要なオーディオストリームの情報を持つオブジェクト       |  |

#### 備考
`StreamDeliver`ディレクティブで送信される`AudioStreamInfoObject`オブジェクトは、既存の[`AudioPlayer.Play`](#Play)ディレクティブで送信された`AudioStreamInfoObject`オブジェクトと重複しないように、一部の内容が省略されることがあります。ストリームを再生する際、`StreamDeliver`ディレクティブと、すでに受信した[`Play`](#Play)ディレクティブの`payload.audioStream`情報を組み合わせて使用する必要があります。

#### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamDeliver",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
        "audioItemId": "5313c879-25bb-461c-93fc-f85d95edf2a0",
        "stream": {
            "token": "b767313e-6790-4c28-ac18-5d9f8e432248",
            "url": "https://musicservice.example.net/b767313e.mp3"
        }
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamRequested`](#StreamRequested)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.StreamRequested -->

### AudioPlayer.StreamRequestedイベント {#StreamRequested}
オーディオストリームを再生するために、ストリーミングのURIなど、追加の情報をCICにリクエストするイベントです。

#### Payload fields
| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `audioItemId`   | string  | オーディオストリームのトークン          |  |
| `audioStream`   | [AudioStreamInfoObject](#AudioStreamInfoObject) | Playディレクティブの`audioItem.stream` |  |

#### 備考
ストリーミングサービスの課金などの理由により、ときには、実際のオーディオストリームの情報の発行を、再生直前に遅延させる必要が生じます。このイベントは、そのようにオーディオストリームの情報を前もって用意してはいけない場合のために設計されたAPIです。クライアントは、このイベントを再生直前より先に送信しないようにする必要があります。

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      "messageId": "198cf12-4020-b98a-b73b-1234ab312806"
    },
    "payload": {
      "audioItemId": "ac192f4c-8f12-4a58-8ace-e3127eb297a4",
      "audioStream": {
        "beginAtInMilliseconds": 0,
        "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
        },
        "token": "TR-NM-4435786",
        "urlPlayable": false,
        "url": "clova:TR-NM-4435786"
      }
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)

<!-- End of the shared content -->

<!-- Start of the shared content: PlaybackController.Pause -->

### PlaybackController.Pauseディレクティブ {#Pause}
クライアントに、再生中のオーディオストリームを一時停止するように指示します。クライアントは、このディレクティブを受信すると、オーディオストリームの再生を一時停止する必要があります。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。このディレクティブから制御対象を確認できます。 | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  | 常時  |

#### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "a85e78b4-44f7-4bea-8a40-66c181b9720f",
      "messageId": "a76c8dfd-c1b6-44f6-a58d-fc8f33c242c1"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

#### 次の項目も参照してください。
* [`AudioPlayer.PlayPaused`](#PlayPaused)

<!-- End of the shared content -->

<!-- Start of the shared content: PlaybackController.Resume -->

### PlaybackController.Resumeディレクティブ {#Resume}
クライアントに、オーディオストリームの再生を再開するように指示します。クライアントは、このディレクティブを受信すると、オーディオストリームの再生を再開する必要があります。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。このディレクティブから制御対象を確認できます。 | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  | 常時  |

#### Message example

```json
//サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

//サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

#### 次の項目も参照してください。
* [`AudioPlayer.PlayResumed`](#PlayResumed)

<!-- End of the shared content -->

<!-- Start of the shared content: PlaybackController.Stop -->

### PlaybackController.Stopディレクティブ {#Stop}
クライアントに、オーディオストリームの再生を停止するように指示します。クライアントは、このディレクティブを受信すると、オーディオストリームの再生を停止する必要があります。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。このディレクティブから制御対象を確認できます。 | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  | 常時  |

#### Message example

```json
//サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

//サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

#### 次の項目も参照してください。
* [`AudioPlayer.PlayResumed`](#PlayResumed)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechFinished -->

### SpeechSynthesizer.SpeechFinishedイベント {#SpeechFinished}
クライアントから、合成音声の再生を完了したことをレポートします。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | 合成音声を識別するためのトークン           | 常時    |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechFinished",
      "messageId": "15472673-49a0-4aa1-8cf0-6355669ea473"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStarted -->

### SpeechSynthesizer.SpeechStartedイベント {#SpeechStarted}
クライアントから、合成音声の再生を開始したことをレポートします。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | 合成音声を識別するためのトークン           | 常時    |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStarted",
      "messageId": "380c805c-0f19-4ed2-84e2-056f2f4016de"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStopped -->

### SpeechSynthesizer.SpeechStoppedイベント {#SpeechStopped}
クライアントから、合成音声の再生を停止したことをレポートします。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | 合成音声を識別するためのトークン           | 常時    |

#### Message example
{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStopped",
      "messageId": "9a511e5c-4f20-413a-94cc-48172fc8710e"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)

<!-- End of the shared content -->

<!-- Start of the shared content: TemplateRuntime.RenderPlayerInfo -->

### TemplateRuntime.RenderPlayerInfoディレクティブ {#RenderPlayerInfo}

CICから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをクライアントに送信し、表示するように指示します。ユーザーがオーディオを再生するようにリクエストすると、クライアントは[`AudioPlayer.Play`](#Play)を受信してメディアを再生します。ディスプレイを持つクライアントは、必要に応じてメディアプレーヤーに再生関連情報を表示する必要があります。その際、[`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)イベントで再生メタデータをCICにリクエストし、`TemplateRuntime.RenderPlayerInfo`ディレクティブを受信します。`TemplateRuntime.RenderPlayerInfo`ディレクティブには、現在再生するメディアコンテンツと、後で再生するメディアコンテンツの再生メタデータが含まれます。クライアントは、`TemplateRuntime.RenderPlayerInfo`ディレクティブの再生メタデータをユーザーに提供して、現在再生しているメディアのメタデータおよび再生リストを表示することができます。

#### Payload fields
| フィールド名       | データ型    | フィールドの説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `displayType`               | string | メディアコンテンツを表示する形式。<ul><li><code>"list"</code>：リストで表示する</li><li><code>"single"</code>：1つのアイテムを表示する</li></ul>       |  |
| `controls[]`                | object array | クライアントがメディアプレーヤーで表示すべきボタンの情報を持つオブジェクト配列です。             |  |
| `controls[].enabled`        | boolean      | `controls[].name`で設定されたボタンを、メディアプレーヤーで有効にするかを示します。<ul><li><code>true</code>：有効にする</li><li><code>false</code>：無効にする</li></ul>  |   |
| `controls[].name`           | string       | ボタンの名前。次のいずれかになります。<ul><li><code>"NEXT"</code>：「次」ボタン</li><li><code>"PLAY_PAUSE"</code>：「再生/一時停止」ボタン</li><li><code>"PREVIOUS"</code>：「前」ボタン</li></ul>  |   |
| `controls[].selected`       | boolean      | メディアコンテンツが選択されているかを示します。この値は、ユーザーの「好き」という概念を表す際に使用することができます。この値が`true`に設定されていたら、ユーザーが好きなアイテムとして登録したコンテンツであることを示します。メディアプレーヤーの関連するUIでそのことを表示する必要があります。<ul><li><code>true</code>：選択</li><li><code>false</code>：未選択</li></ul> |   |
| `controls[].type`           | string       | ボタンのタイプ。現在、`"BUTTON"`のみ使用します。  |  |
| `playableItems[]`           | object array | 再生できるメディアコンテンツのリストを持つオブジェクト配列です。このフィールドは、空の配列の場合があります。  |  |
| `playableItems[].artImageUrl`  | string    | メディアコンテンツ関連画像のURI。アルバムのジャケット画像や関連アイコンなどの画像があるURIです。      | 条件付き |
| `playableItems[].controls[]`                | object array  | 特定のメディアコンテンツを再生するとき、表示すべきボタンの情報を持つオブジェクト配列です。このオブジェクト配列は省略できます。  | 条件付き |
| `playableItems[].controls[].enabled`        | boolean      | `playableItems[].controls[].name`で設定されたボタンを、メディアプレーヤーで有効にするかを示します。<ul><li><code>true</code>：有効にする</li><li><code>false</code>：無効にする</li></ul>  |   |
| `playableItems[].controls[].name`           | string       | ボタンまたはコントロールUIの名前。次のいずれかになります。<ul><li><code>"BACKWARD_15S"</code>：「15秒巻き戻しする」ボタン</li><li><code>"BACKWARD_30S"</code>：「30秒巻き戻しする」ボタン</li><li><code>"BACKWARD_60S"</code>：「60秒巻き戻しする」ボタン</li><li><code>"FORWARD_15S"</code>：「15秒早送りする」ボタン</li><li><code>"FORWARD_30S"</code>：「30秒早送りする」ボタン</li><li><code>"FORWARD_60S"</code>：「60秒早送りする」ボタン</li><li><code>"NEXT"</code>：「次へ」ボタン</li><li><code>"PLAY_PAUSE"</code>：「再生/一時停止」ボタン</li><li><code>"PREVIOUS"</code>：「前へ」ボタン</li><li><code>"PROGRESS_BAR"</code>：進行状況バー</li><li><code>"REPEAT"</code>：「リピート」ボタン</li><li><code>"SUBSCRIBE_UNSUBSCRIBE"</code>：「購読/購読解除」ボタン</li></ul>  |   |
| `playableItems[].controls[].selected`       | boolean      | メディアコンテンツが選択されているかを示します。この値は、ユーザーの「好き」という概念を表す際に使用することができます。この値が`true`に設定されていたら、ユーザーが好きなアイテムとして登録したコンテンツであることを示します。メディアプレーヤーの関連するUIでそのことを表示する必要があります。<ul><li><code>true</code>：選択</li><li><code>false</code>：未選択</li></ul> |   |
| `playableItems[].controls[].type`           | string       | ボタンのタイプ。現在、`"BUTTON"`のみ使用します。  |  |
| `playableItems[].headerText`       | string        | 主に、現在の再生リストのタイトルを表すテキストフィールド                                                | 条件付き  |
| `playableItems[].isLive`           | boolean       | リアルタイムのコンテンツかどうかを示す値。<ul><li><code>true</code>：リアルタイムのコンテンツ</li><li><code>false</code>：リアルタイムのコンテンツではない</li></ul><div class="note"><p><strong>メモ</strong></p><p>リアルタイムのコンテンツは、リアルタイムのコンテンツであることを表すアイコン（例：liveアイコン）を表示する必要があります。</p></div>  | 条件付き  |
| `playableItems[].lyrics[]`         | object array  | 歌詞のデータを持つオブジェクト配列。                                                            | 条件付き  |
| `playableItems[].lyrics[].data`    | string        | 歌詞のデータ。このフィールドと`playableItems[].lyrics[].url`フィールドのうち、1つは存在します。              | 条件付き  |
| `playableItems[].lyrics[].format`  | string        | 歌詞データの形式。<ul><li><code>"LRC"</code>：<a href="https://en.wikipedia.org/wiki/LRC_(file_format)" target="_blank">LRC形式</a></li><li><code>"PLAIN"</code>：テキスト形式</li></ul>  |   |
| `playableItems[].lyrics[].url`     | string        | 歌詞データのURI。このフィールドと`playableItems[].lyrics[].data`フィールドのうち、1つは存在します。        | 条件付き  |
| `playableItems[].showAdultIcon`    | boolean       | 成人向けコンテンツを示すアイコンを表示するかどうか。<ul><li><code>true</code>：表示する。</li><li><code>false</code>：表示しない。</li></ul>   |   |
| `playableItems[].titleSubText1`    | string        | 主にアーティスト名を表すテキストフィールド                                                          |  |
| `playableItems[].titleSubText2`    | string        | 主にアルバム名を表すサブテキストフィールド                                                      | 条件付き |
| `playableItems[].titleText`        | string        | 現在のオーディオコンテンツのタイトルを表すテキストフィールド                                                         |   |
| `playableItems[].token`            | string        | メディアコンテンツのトークン                                                                     |  |
| `provider`                         | object        | メディアコンテンツ提供元の情報を持つオブジェクト                                                         | 条件付き |
| `provider.logoUrl`                 | string        | メディアコンテンツ提供元のロゴ画像のURIこのフィールドまたはフィールドの値がなかったり、ロゴ画像を表示できない場合、`provider.name`フィールド内のメディアコンテンツ提供元の名前を表示する必要があります。 | 条件付き |
| `provider.name`                    | string        | メディアコンテンツ提供元の名前                                                                   |   |
| `provider.smallLogoUrl`            | string        | メディアコンテンツ提供元の小さなロゴ画像のURI                                                | 条件付き |

#### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RenderPlayerInfo",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
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
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUE="
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
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)

<!-- End of the shared content -->

<!-- Start of the shared content: TemplateRuntime.RequestPlayerInfo -->

### TemplateRuntime.RequestPlayerInfoイベント {#RequestPlayerInfo}
クライアントから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをCICにリクエストします。

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`        | string  | 再生メタデータを取得するとき、開始の基準となるオーディオストリームのトークン。 |  |
| `range`        | object  | 再生メタデータの範囲を指定するオブジェクト。このフィールドが使用されていない場合、クライアントは任意の数のメタデータを受信します。   | 任意  |
| `range.before` | number  | 基準となるメディアコンテンツから、n個前以前の再生リストに含まれた再生メタデータをリクエストします。  | 任意  |
| `range.after`  | number  | 基準となるメディアコンテンツから、n個以降の再生リストに含まれた再生メタデータをリクエストします。例えば、`range.before`フィールドの値を指定しないで、`range.after`を`5`に設定すると、基準のメディアコンテンツを含めて、合計6つのメディアコンテンツに該当する再生メタデータを受信します。 | 任意  |

#### Message example

{% raw %}

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RequestPlayerInfo",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)

### AudioStreamInfoObject {#AudioStreamInfoObject}
再生するオーディオストリームのストリーミング情報を持つオブジェクトです。クライアントに対して再生するストリーミングの情報を送信したり、クライアントがCICに対して、現在再生しているコンテンツのストリーミング情報を送信するとき使用します。

#### Object fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:-------------:|
| `beginAtInMilliseconds`  | number | 再生を開始するオフセット。ミリ秒単位で、この値が指定されている場合、クライアントは、そのオーディオストリームを指定されたオフセットから再生する必要があります。この値が0に設定されている場合、ストリームを最初から再生します。          |  |
| `customData`             | string | 現在のストリームに関連して、任意の形式を持つメタデータ情報。特定のカテゴリに分類されたり、定義されないストリーミング情報は、このフィールドに含まれるか、または入力される必要があります。オーディオストリーム再生のコンテキストに必要な追加の値を、サービスプロバイダーがカスタムで追加できます。<div class="warning"><p><strong>警告</strong></p><p>クライアントは、このフィールドの値を任意に使用してはいけません。問題が発生する恐れがあります。また、このフィールドの値は、オーディオ再生状態を送信する際、<a href="{{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Context_Objects.{{ book.DocMeta.FileExtensionForExternalLink }}#PlaybackState">PlaybackStateコンテキスト</a>の`stream`フィールドにそのまま含まれる必要があります。</p></div> | 任意/条件付き  |
| `durationInMilliseconds` | number | オーディオストリームの再生時間。クライアントは、`beginAtInMilliseconds`フィールドに指定されている再生のオフセットから、このフィールドに指定されている再生時間だけ、そのオーディオストリームをシークおよび再生できます。例えば、`beginAtInMilliseconds`フィールドの値が`10000`で、このフィールドの値が`60000`の場合、そのオーディオストリームの10秒から70秒までの区間を再生およびシークすることができます。ミリ秒単位です。   | 任意/条件付き  |
| `format`                 | string  | メディアフォーマット（MIMEタイプ）。このフィールドから、HLS（HTTP Live Streaming）コンテンツかどうかを確認できます。次の値を持ちます。デフォルト値は`"audio/mpeg"`です。<ul><li><code>"audio/mpeg"</code></li><li><code>"audio/mpegurl"</code></li><li><code> "audio/aac"</code></li><li><code>"application/vnd.apple.mpegurl"</code></li></ul><div class="note"><p><strong>メモ</strong></p><p>HLSでコンテンツを提供するExtensionの開発者は、<a href="mailto:{{ book.ServiceEnv.ExtensionAdminEmail }}">{{ book.ServiceEnv.ExtensionAdminEmail }}</a>までご連絡ください。</p></div>   | 任意/条件付き  |
| `progressReport`         | object  | 再生が開始してから、再生状態をレポートするタイミングを指定するオブジェクト                                                  | 任意/条件付き |
| `progressReport.progressReportDelayInMilliseconds`    | number | 再生が開始してから、指定された時間が経過した後に、再生状態をレポートするように指定する値です。ミリ秒単位で、このフィールドの値はnullの場合があります。  | 任意/条件付き |
| `progressReport.progressReportIntervalInMilliseconds` | number | 再生中に、指定された間隔ごとに再生状態をレポートするように指定する値です。ミリ秒単位で、このフィールドの値はnullの場合があります。        | 任意/条件付き |
| `progressReport.progressReportPositionInMilliseconds` | number | 再生中に、指定された再生位置を経過する度に、再生状態をレポートするように指定する値です。ミリ秒単位で、このフィールドの値はnullの場合があります。    | 任意/条件付き |
| `token`                  | string  | オーディオストリームのトークン。<div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは2048バイトです。</p></div>                          |  |
| `url`                    | string  | オーディオストリームのURL。<div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは2048バイトです。</p></div>                              |  |
| `urlPlayable`            | boolean | `url`フィールドのオーディオストリームのURIがすぐに再生できるかを示す値。<ul><li><code>true</code>：すぐに再生できるURI</li><li><code>false</code>：すぐに再生できないURI。<a href="#StreamRequested"><code>AudioPlayer.StreamRequested</code></a>イベントでオーディオストリームの情報を追加でリクエストする必要があります。</li></ul>        |  |

#### 備考
* クライアントは、`beginAtInMilliseconds`と`durationInMilliseconds`フィールドに指定されている区間に対してストリームの再生を完了すると、[`AudioPlayer.PlayFinished`](#PlayFinished)イベントをCICに送信します。

#### Object Example
{% raw %}

```json
// すぐに再生できるオーディオストリームのURIが含まれたオブジェクト
{
  "beginAtInMilliseconds": 0,
  "episodeId": 22346122,
  "playType": "NONE",
  "podcastId": 12548,
  "progressReport": {
    "progressReportDelayInMilliseconds": null,
    "progressReportIntervalInMilliseconds": 60000,
    "progressReportPositionInMilliseconds": null
  },
  "url": "https://streaming.example.com/file/12548/22346122",
  "urlPlayable": true
}

// すぐに再生できないオーディオストリームのURIが含まれたサンプル
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
      "progressReportDelayInMilliseconds": null,
      "progressReportIntervalInMilliseconds": null,
      "progressReportPositionInMilliseconds": 60000
  },
  "token": "TR-NM-4435786",
  "urlPlayable": false,
  "url": "clova:TR-NM-4435786"
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayFinished`](#PlayFinished)
* [`AudioPlayer.StreamRequested`](#StreamRequested)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlaybackState -->

## オーディオコンテンツ再生関連のコンテキスト {#ContextObjectforAudioPlayback}
クライアントがClovaに[CIC API](#CICAPIforAudioPlayback)イベントを送信するとき、以下のようなさまざまなコンテキストが含まれます。そのうち、オーディオコンテンツの再生に関連するコンテキストは、[`AudioPlayer.PlaybackState`](#PlaybackState)です。

### AudioPlayer.PlaybackState {#PlaybackState}
`AudioPlayer.PlaybackState`は、現在再生しているか、または最後に再生したオーディオの情報をCICにレポートするときに使用されるメッセージ形式です。

#### Object structure

{% raw %}
```json
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "offsetInMilliseconds": {{number}},
    "playerActivity": {{string}},
    "repeatMode": {{string}},
    "stream": {{AudioStreamInfoObject}},
    "totalInMilliseconds": {{number}}
  }
}
```
{% endraw %}

#### Payload fields

| フィールド名       | データ型    | フィールドの説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 最近再生したメディアアイテムの最後の再生ポイント（オフセット）。ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。                                                  | 任意 |
| `playerActivity`       | string | プレイヤーの状態を示す値です。次のような値を持ちます。<ul><li><code>"IDLE"</code>：非アクティブ状態</li><li><code>"PLAYING"</code>：再生中</li><li><code>"PAUSED"</code>：一時停止状態</li><li><code>"STOPPED"</code>：停止状態</li></ul> |  |
| `repeatMode`           | string  | リピート再生モード<ul><li><code>"NONE"</code>：リピート再生しない</li><li><code>"REPEAT_ONE"</code>：一曲リピート再生</li></ul>                                                   |   |
| `stream`               | [AudioStreamInfoObject](#AudioStreamInfoObject) | 再生中のオーディオの詳細情報が保存されているオブジェクト`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。[`AudioPlayer.Play`](#Play)または[`AudioPlayer.StreamDeliver`](#StreamDeliver)ディレクティブで送信されたメディアアイテムの情報（`stream`オブジェクト）の値を入力します。 | 任意 |
| `totalInMilliseconds`  | number | 最近再生したメディアアイテムの全長。[`AudioPlayer.Play`](#Play)ディレクティブで送信されたオーディオストリームの情報（[AudioStreamInfoObject](#AudioStreamInfoObject)）に`durationInMilliseconds`フィールド値がある場合、このフィールドに入力します。ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。                                                               | 任意 |

#### Object example

{% raw %}

```json
// Case 1：再生が停止された状態
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "offsetInMilliseconds": 10000,
    "totalInMilliseconds": 300000,
    "playerActivity": "STOPPED",
    "repeatMode": "NONE",
    "stream": {
      "beginAtInMilliseconds": 0,
      "progressReport": {
        "progressReportDelayInMilliseconds": null,
        "progressReportIntervalInMilliseconds": null,
        "progressReportPositionInMilliseconds": 60000
      },
      "token": "TR-NM-17740107",
      "url": "clova:TR-NM-17740107",
      "urlPlayable": false
    }
  }
}

//サンプル2：プレイヤーが非アクティブになっている状態
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "playerActivity": "IDLE",
    "repeatMode": "NONE"
  }
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamDeliver`](#StreamDeliver)
* [`AudioPlayer.StreamRequested`](#StreamRequested)

<!-- End of the shared content -->

{% endif %}
