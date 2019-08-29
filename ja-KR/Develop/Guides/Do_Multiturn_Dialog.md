# マルチターン対話をする

CEKから渡されたユーザーのリクエスト（[`IntentRequest`](/Develop/Guides/Build_Custom_Extension.md#HandleIntentRequest)）に、Custom Extensionがサービスを提供したり、または動作するために必要な情報がすべて含まれていないことがあります。また、シングルターンの対話では、1回の発話でユーザーのリクエストを受け付けることが難しい場合もあります。その場合、Custom Extensionは、ユーザーから足りない情報を引き出すためにマルチターンの対話を行うことができます。

例えば、ユーザーが「ペパロニピザを頼んで」と発話したと仮定します。それを受け、CEKは次のようなリクエストメッセージを送信します。

{% raw %}
```json
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
    ...
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
```
{% endraw %}

Custom Extensionがピザの種類だけでなく、注文する数量に関する情報を必要とすることがあります。その際、[レスポンスメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage)の`response.shouldEndSession`フィールドを`false`に設定すると、マルチターン対話で足りない情報を確認することができます。また、ユーザーが先に入力した情報を`sessionAttributes`フィールドにキー（key）-値（value）の形で保存することもできます。

以下のようにレスポンスを返すことで、ユーザーがすでにリクエストした`intent`フィールドと`pizzaType`の情報を保存するようにClovaにリクエストすることができます。また、ユーザーに数量に関する追加の情報を求めることができます。

{% raw %}
```json
{
  "version": "0.1.0",
  "sessionAttributes": {
    "intent": "OrderPizza",
    "pizzaType": "ペパロニ"
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
```
{% endraw %}

ユーザーが必要な数量まで話すと、Clovaプラットフォームは、次のように解析された数量情報と一緒に、保存していた`sessionAttributes`オブジェクトの情報を[リクエストメッセージ](/Develop/References/Custom_Extension_Message.md#CustomExtRequestMessage)の`session.sessionAttributes`フィールドに含めて再送信します。その際、追加に送信されたメッセージは前のメッセージと同じ`session.sessionId`値を持ち、Custom Extensionは受信した追加の情報を使用して次の動作を行います。

{% raw %}
```json
{
  "version": "0.1.0",
  "session": {
    "new": false,
    "sessionAttributes": {
        "intent": "OrderPizza",
        "pizzaType": "ペパロニ"
    },
    "sessionId": "a29cfead-c5ba-474d-8745-6c1a6625f0c5",
    "user": {
      "userId": "V0qe",
      "accessToken": "XHapQasdfsdfFsdfasdflQQ7"
    }
  },
  "context": {
    ...
  },
  "request": {
    "type": "IntentRequest",
    "intent": {
      "name": "AddInfo",
      "slots": {
        "pizzaAmount": {
          "name": "pizzaAmount",
          "value": "2"
        }
      }
    }
  }
}
```
{% endraw %}

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>Extensionが<a href="#HandleSessionEndedRequest"><code>SessionEndedRequest</code>タイプのリクエスト</a>を受信すると、マルチターンの対話は終了します。<code>SessionEndedRequest</code>タイプのリクエストを受信してからは、Extensionからどんな応答（使用終了のあいさつなど）が返されても、CEKで無視されます。</p>
</div>
