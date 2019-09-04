# Engaging in multi-turn dialogues

The user request information received from CEK ([`IntentRequest`](/Develop/Guides/Build_Custom_Extension.md#HandleIntentRequest)) may be insufficient for the custom extension to provide services or perform operations. Or the conversation may be a single-turn dialogue, which cannot handle many user requests at once. At this time, the custom extension can perform a multi-turn dialogue to receive missing information from the user.

The example below assumes that the user said "Order pepperoni pizza," so CEK sends a request message as follows:

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
          "value": "Pepperoni"
        }
      }
    }
  }
}
```
{% endraw %}

The custom extension may require not only the pizza type but also the quantity of order. If you set the `response.shouldEndSession` field of the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage) to `false`, a multi-turn dialogue can be attempted in order to check missing information. Also, you can store the information sent by the user earlier in the `sessionAttributes` field as a key-value format.

If a response is made as below, the extension can request Clova to store the `intent` field and `pizzaType` information that the user has previously requested and request the quantity information and any other additional information from the user.

{% raw %}
```json
{
  "version": "0.1.0",
  "sessionAttributes": {
    "intent": "OrderPizza",
    "pizzaType": "Pepperoni"
  },
  "response": {
    "outputSpeech": {
      "type": "SimpleSpeech",
      "values": {
          "type": "PlainText",
          "lang": "ko",
          "value": "How many boxes of pizza do you want to order?"
      }
    },
    "card": {},
    "directives": [],
    "shouldEndSession": false
  }
}
```
{% endraw %}

When the user supplies the quantity value, the Clova platform sends the stored `sessionAttributes` object information again by including it in the `session.sessionAttributes` field of [request message](/Develop/References/Custom_Extension_Message.md#CustomExtRequestMessage) together with the analyzed quantity information. When sent, this additional message gets to have the same `session.sessionId` value as the previously sent message, and the custom extension can perform the action below using the additionally received information.

{% raw %}
```json
{
  "version": "0.1.0",
  "session": {
    "new": false,
    "sessionAttributes": {
        "intent": "OrderPizza",
        "pizzaType": "Pepperoni"
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
  <p><strong>Tip!</strong></p>
  <p>Multi-turn dialogues can end anytime once the extension receives a <a href="#HandleSessionEndedRequest"><code>SessionEndedRequest</code> type request</a>. After receiving the <code>SessionEndedRequest</code> type request, CEK will ignore all responses sent by the extension such as a goodbye message.</p>
</div>
