# Designing custom extensions

When creating a custom extension, you must first decide which user requests your technology and services will handle, and which responses they will return on Clova. This document describes how to design custom extensions. Note that the Clova functionalities are provided to the users in the name of a "skill" and you must implement a custom extension to provide skills to users.

It is possible to create a custom extension for looking up web service information, shopping and delivery services, interactive games, broadcasts, real-time briefings, and even for other voice-initiated activities or services. When designing a custom extension, complete the following steps: Note that the details covered here are only the basic recommendations for designing the custom extension with examples. You can further design and implement the custom extension according to your own business experience and service characteristics.

* [Setting goals](#SettingGoal)
* [Writing user scenario scripts](#MakeUseCaseScenarioScript)
* [Defining the skill name](#DefineInvocationName)
* [Defining an interaction model](#DefineInteractionModel)
* [Deciding the sound output type](#DecideSoundOutputType)
* [Continuous updates](#ContinuousUpdate)

## Setting goals {#SettingGoal}

The first thing you need to do when designing a custom extension is to set the goals of the custom extension. Setting goals of a custom extension is a process of deciding what and how to deliver to users and the method that will be used for delivering it. This process becomes the basis for anticipating the functions to provide to users later on and the user scenarios for using the functions. It may be a single basic and abstract goal for the custom extension, as shown below.

```
Provide a pizza delivery service to users.
```

This goal can be written again into a series of detailed, more specific goals. See the following items for defining detailed goals:

* Write the detailed goals from the user point of view.
* Include the details on the ways that the user will call the custom extension.
* Include the prerequisites fulfilling the detailed goals and the outcomes that can be achieved. Prerequisites may include:
  - Actions or states required in advance
  - Functions or resources (e.g. GPS, camera, or microphone) required by the custom extension
  - Information on external services or platforms (e.g. contacts on a mobile device contacts or social media account information)
* Check that the collection of detailed goals meets the scope of custom extension goals.
* Each detailed goal is recommended to be written on the level of a single user action, a unit that is classified and processed in the service.

Below is an example of detailed goals for a pizza delivery service.

| Detailed goal ID | Classification                | Goal                                                            |
|:----------:|-----------------------|-----------------------------------------------------------------|
| #1         | Call the service           | A user can start using the pizza delivery service by saying "Start Pizzabot" or "On Pizzabot _[execution commands registered by extension]_"    |
| #2         | Usage suggestions or recommendations   | Once the pizza delivery service starts, the user can receive information on the next or the anticipated action in the pizza delivery process.       |
| #3         | Menu lookup and selection     | The user can view the menu and choose their pizza.                                                                            |
| #4         | Brand lookup and selection   | The user can select a pizza brand of their choice.                                                                                   |
| #5         | Order and payment          | The user can order pizza if information on the pizza type, quantity, and delivery address exist.                                              |
| #6         | Usage suggestions or recommendations   | If the user selects a pizza brand of their choice, they can get suggestions for the pizza, the delivery destination, and the payment method based on their latest order information.               |
| #7         | Usage suggestions or recommendations   | If the user requests a different menu, they can get a menu recommendation from the extension.                                     |
| #8         | Order and payment          | For payment, the user can use the camera to scan and apply coupon discounts.                                                                |
| #9         | Checking delivery             | Once the order is complete, the user checks the preparation and delivery status.                                          |
| #10        | End service           | The user can end the service after completing the desired task.                                                                |
| ...        | ...                   | ...                                                                                                                      |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The detailed goals prepared this way become the basis for <a href="#MakeUseCaseScenarioScript">writing user scenario scripts</a> or defining the <a href="#DefineInteractionModel">interaction model</a>. Also, this information is required for <a href="/DevConsole/Guides/Deploy_Custom_Extension.md#InputDeploymentInfo">deploying the extension</a> and will be used as a basis to <a href="/DevConsole/Guides/Deploy_Custom_Extension.md#RequestExtensionSubmission">review</a> the proper operation of the custom extension.</p>
</div>

## Writing user scenario scripts {#MakeUseCaseScenarioScript}

The user scenario script anticipates the conversation between the user and Clova in advance. By predicting the conversation between the user and Clova that would be exchanged in various user scenarios based on detailed goals, the level of convenience and flow of the service can be examined. You should write the anticipated user scenario scripts based on the detailed goals defined in [Setting goals](#SettingGoal). This may be used again when registering an [interaction model](#DefineInteractionModel) later on.

See the following recommendations for writing user scenario scripts:

* Write in a colloquial style rather than in a written style.
* Make suggestions for the next action or make service usage recommendations to the user.
* Avoid repetitive/redundant expressions.
* Keep in mind that an unexpected user request or situation can occur at any point.

Next is an example of writing a script by dividing the usage scenario into multi-turn requests and single-turn requests.
* Multi-turn request scenario

| Speaker           | Sample utterance                                                                   | Relevant detailed goal     |
|-------------------|---------------------------------------------------------------------------|----------------|
| User     | Start Pizzabot.                                                                                                    | #1             |
| Extension | Hello, this is Pizzabot. What can I do for you?                                                                             | #2             |
| User      | What kind of pizzas do you have?                                                                                                    | #3             |
| Extension | I have combination pizza, pepperoni pizza, and super supreme pizza. What would you like?                                                   | #2, #3         |
| User      | I'll order the combination pizza.                                                                                             | #3             |
| Extension | You can order from XX Pizza and YY Pizza. Which one would you like?                                                                  | #2, #4         |
| User      | XX Pizza.                                                                                                     | #4             |
| Extension  | I can order one combination pizza and one 1.5 liter coke from XX Pizza in XX sent to 111, AA-dong. The total is 15 USD and you can pay the delivery man. Shall I order it? | #2, #6         |
| User      | Tell me another menu.                                                                                                   | #7             |
| Extension | I can order one super supreme pizza and one 1.5 liter coke sent to 111, AA-dong. The total is 20 USD and you can pay the deliveryman. Shall I order it?          | #2, #7         |
| User     | Yes.                                                                                                             | #5             |
| Extension | I have made your order.                                                                                                | #8             |
| User     | Exit Pizzabot.                                                                                                      | #10            |

* Single-turn request scenario

| Speaker   | Sample utterance                                              | Relevant detailed goal  |
|-----------|------------------------------------------------------|-------------|
| User      | Update me on the order from Pizzabot.                               | #1, #9      |
| Extension | It's on its way. Please wait a little while longer.                       | #9          |

## Defining the skill name {#DefineInvocationName}

When creating a new custom extension, you must define the **skill name** and **invocation name**. The **skill name** is the name of the custom extension that is provided on the Skill Store. The **invocation name** is the name that the user says to Clova to start the custom extension.

The **skill name** and **invocation name** do not have to be the same. However, it is preferable to use the same name or a similar name to reduce confusion.

The **skill name** and **invocation name** must satisfy the following conditions:

<table>
  <thead>
    <tr>
      <th style="width:40%">Condition</th><th style="width:60%%">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>No one-word names or universally used words</td>
      <td>One-word names cannot be used unless they are the names of private brands or services. Also, universally used words such as "sports news," are not permitted. It is recommended that you combine common words with another name such as the name of the developer, developer company, service, or brand (e.g., "Taro's sports news").</td>
    </tr>
    <tr>
      <td>No words that refer to the name of a person or place</td>
      <td>The name of a person or place cannot be used. In some cases, the use may be permitted after a review if only a part of the name of a person or place is used.
        <ul>
          <li>Not permitted: "King Sejong the Great," "Yi Sun-shin," "Seoul City," "Gangnam District"</li>
          <li>Permitted after review: "King Sejong's Korean class," "Hotspots in Seoul"</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>No words or expressions that affect the functions of Clova</td>
      <td>Names that contain any expressions that affect the functions of Clova cannot be used.
        <ul>
          <li>Clova invocation words: "Clova," "Hey Clova," "Hey Sally," "Hey Jessica," "Hey Jjanggu"</li>
          <li>Default Clova skill invocation words: Expressions to execute and invoke basic Clova skills such as "Today's weather" and "News headlines"</li>
          <li>Phrases that start or end the <strong>invocation name</strong>: "Start," "Stop"</li>
          <li>Prepositions used with the <strong>invocation name</strong>: "To," "From"</li>
        </ul>
        <div class="note">
          <p><strong>Note!</strong></p>
          <p>The <strong>invocation name</strong> must match well with the prepositions that are used with the name.</p>
        </div>
      </td>
    </tr>
    <tr>
      <td>No names that are the same as or similar to another skill</td>
      <td>The names already in use by another skill or similar names cannot be used.</td>
    </tr>
    <tr>
      <td>No name that could be easily misunderstood</td>
      <td>The following names that may be misleading cannot be used:
        <ul>
          <li>Names that can be mistaken for those of a third party {{ book.ServiceEnv.OrientedService }} or an affiliated company (e.g. Naver fishing master)</li>
          <li>The <strong>skill name</strong> and <strong>invocation name</strong> are very different to the point of having contradicting meanings (e.g. The skill name is "Cat meowing" and the invocation name is "Dog barking")</li>
          <li>The name and the provided content are different (e.g. The name is "Cat meowing" but the provided content is a dog barking)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>No names that violate the Terms and Conditions</td>
      <td>The names must comply with the <a href="{{ book.ServiceEnv.CEKTermsOfUseURI }}" target="_blank">Clova extensions kit Terms and Conditions</a>. Names that infringe the rights of a third party or use obscene expressions cannot be used.</td>
    </tr>
    <tr>
      <td>Other precautions</td>
      <td>Other precautionary conditions for names are as follows:
        <ul>
          <li>Proper nouns are allowed as an exception such as private brand names, intellectual property, names, and places.</li>
          <li>If possible, it is recommended that you use an <strong>invocation name</strong> that can be easily pronounced or that does not have an alternative pronunciation.</li>
          <li>To help users understand how to use the extension, you can include the <strong>invocation name</strong> in parentheses next to the <strong>skill name</strong>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

<div class="note">
<p><strong>Note!</strong></p>
<p>Conditions of the skill name are subject to change at any time. Any existing names can be rejected due to changes in policy. Thank you for your understanding in this matter, and we apologize for any inconvenience. If any of the conditions are unclear, let us know when you <a href="/DevConsole/Guides/Deploy_Custom_Extension.md#RequestExtensionSubmission">submit a request for review</a>.</p>
</div>

## Defining an interaction model {#DefineInteractionModel}

An interaction model in Clova is a set of rules to convert the voice user request into a standardized format (JSON) to be processed in the custom extension. For example, if a custom extension is providing a pizza delivery service, a user might say "Order two boxes of pepperoni pizza." The interaction model defines the rules for converting such requests into the format required for providing the service (JSON) as shown below.

![](/Design/Assets/Images/Extension_Design-Interaction_Model_Analysis_Diagram.png)

Before defining the interaction model in the Clova developer console, there is a need to understand and design the model first. If the interaction model is registered in the Clova developer console without such a process, the task efficiency may drop or the user requests may not be carried out as intended. In order to make an interaction model that accurately identifies the real intention of users; you must first understand the following content before applying it into creating an interaction model.

* [Intent](#Intent)
* [Slot](#Slot)
* [Sample utterance](#UtteranceExample)

### Intent {#Intent}

Intent is a category used for processing user requests. It is usually classified using the **verb** factor in a user's speech. Then it is divided into a custom intent or built-in intent.

* [Built-in intent](#BuiltinIntent)
* [Custom intent](#CustomIntent)

#### Built-in intent {#BuiltinIntent}

The built-in intent is declared by the Clova platform for shared use on common user request types. The following built-in intents are predefined as commonly invoked intents:

| Built-in intent              | Intent                    | Sample response utterance                      |
|-----------------------------------|-------------------------|------------------------------------------------|
| `Clova.CancelIntent`              | Undo action request          | "Cancel," "Cancel please"                             |
| `Clova.GuideIntent`               | Help request             | "What can you do?", "Tell me how to use it"          |
| `Clova.NextIntent`                | Next content request        | "Next," "Play the next song"                        |
| `Clova.NoIntent`                  | Negative response (e.g. No)   | "No," "No, thank you," "Absolutely not"                       |
| `Clova.PauseIntent`               | Pause request.     | "Pause for a moment," "Pause."                        |
| `Clova.PreviousIntent`            | Previous content request        | "Previous," "Play the previous song"                       |
| `Clova.RequestAlternativesIntent` | Request new content playback | "Try something else", "Play something else"             |
| `Clova.ResumeIntent`              | Resume play request          | "Play again," "Play the song again"                 |
| `Clova.StopIntent`                | Cancel play request          | "Stop"                                    |
| `Clova.YesIntent`                 | Positive response (e.g. Yes.)      | "Yes," "Sure," "All right," "Certainly," "Okay" |

#### Custom intent {#CustomIntent}

Unlike the built-in intent, custom intent defines the type of user request specialized for the service to be provided. The specification of the custom intent include the following content:
* What type of user requests exist in the service?
* What kind of information ([Slot](#Slot)) is required for each type of user request?
* What are the [Sample utterance](#UtteranceExample) in each type of user request?

Continuing with the example of the pizza delivery service, the service would contain the following request types:

* Request to retrieve menu
* Request to order
* Request to update delivery status

Based on this example, we can see that defining the interaction model of a pizza delivery service (extension) is declaring the intent list, such as the retrieve menu intent, order intent, update delivery status intent, and also listing the possible utterances and information (slot) required by each intent. Therefore, **the first thing to do when defining an interaction model is to define and list the type of requests that will be processed by the custom extension.** This also becomes the standard for dividing the business logic–in other words, the branch of program–when developing a custom extension.

![](/Design/Assets/Images/Extension_Design-Design_Interaction_Model.png)

**Once we have divided the type of user requests, we must name each type.** This will later on become the name of the intent. The "order intent" of a pizza delivery service is an abstract concept and it must be declared as a specific name that can be known by the custom extension– an identifiable string. For example, "order intent" can be declared a name like "OrderPizza."

Then, you must **define what information ([Slot](#Slot)) is to be recognized** from the utterance of the "OrderPizza" intent and **list various [sample utterances](#UtteranceExample)** on the types of user utterances that can be processed.

### Slot {#Slot}

A slot is the information acquired from a user utterance and the **noun** factor used in the utterance can become a slot. When defining [custom intent](#Intent), you must define the slot required by the corresponding intent. To explain this more by comparing it with software development, the intent is a function or handler to process a specific type of user request while the slots are parameters required for this function or handler. From the utterance, "Order two boxes of pepperoni pizza," you can see that information on pizza type "pepperoni pizza" and quantity "two" are required to process the "OrderPizza" intent. Therefore, you must identify the information (slot) needed before defining the intent.

When declaring a slot, you must classify the type of information in the slot which is called slot type. The slot type is comprised of built-in slot type and custom slot type.

* [Built-in slot type](#BuiltinSlotType)
* [Custom slot type](#CustomSlotType)

#### Built-in slot type {#BuiltinSlotType}

The built-in slot type is an information type pre-defined by Clova which defines information expression that can be universally used in all services (extension). The built-in slot type is mainly used for recognizing information such as the time, place, or number. For the above utterance, a built-in slot type can be used to recognize the information of "two boxes." Clova provides the following built-in slot types:

| Built-in slot type | Description                                            |
| ----------------------|------------------------------------------------|
| `CLOVA.DATETIME`      | Information indicating date and time (e.g., "10 minutes 30 seconds," "9 am," "1 hour ago," "12 pm," "Noon," "August 4, 2017," "Last day of the previous month") |
| `CLOVA.DURATION`      | Information indicating a period of time (e.g., "One day," "Overnight," "One month," "Next week," "Weekend"). |
| `CLOVA.NUMBER`        | Information indicating numbers including quantity nouns (e.g., "Once," "7 people," "One," "30 years old," "Around 8," "16 slots"). |
| `CLOVA.RELATIVETIME`  | Information indicating relative time expressions (e.g., "From now on," "Later," "In a bit," "Just now," "Earlier"). |
| `CLOVA.UNIT`          | Information indicating unit expressions (e.g., "374 square meters," "100 MB," "25 miles"). |
| `CLOVA.ORDER`        | Information indicating sequencing expressions (e.g., "Next," "Front," "Before," "Last," "This," "Previous"). |
| `CLOVA.KO_ADDRESS_[Unit for administrative district]` | This information indicates the place names that are called according to the domestic unit of administrative districts. You can find the unit of administrative districts provided by Clova in the Clova developer console. |
| `CLOVA.WORLD_COUNTRY` | Information indicating worldwide country names (e.g., "Ghana," "Japan," "Korea," "France"). |
| `CLOVA.WORLD_CITY`   | Information indicating worldwide city names (e.g., "New York," "Paris," "London"). |
| `CLOVA.CURRENCY`      | Information indicating currency (e.g., "Yuan," "Yen," "Dollar," "Russian money," "British currency"). |
| `CLOVA.OFFICIALDATE ` | Information indicating holidays, national holidays, and memorial days (e.g., "Onset of Spring," "New Year's Day," "Buddha's Birthday," "Independence Day"). |

#### Custom slot type {#CustomSlotType}

The custom slot type is an information type specialized to the provided service (extension) and is mainly comprised of proper nouns or nouns. For the aforementioned utterance, the "OrderPizza" intent must identify the relevant information (slot) for the pizza type from user utterance, but there is also a high possibility that the pizza type expressions will be used only in services related to pizza. Therefore, you can define a custom slot type like "PIZZA_TYPE" and declare various items in "PIZZA_TYPE" such as "pepperoni pizza," "combination pizza," and "cheese pizza," which can be ordered from the pizza delivery service.

In a sentence however, these items can be expressed in various ways with a same or similar meaning. "Barbecue pizza" has the same meaning as "BBQ pizza" and long names, such as "shrimp golden crust pizza," may be shortened to "shrimp gold-crust pizza." Therefore, there is a need to not only declare the items classified by the concept but also the representative term for each item and its synonyms when declaring custom slot types. This allows for converting the various synonyms used in the user utterance to the representative term during the recognition process and helps to receive a unified value of information under a similar concept for when the custom extension is handling the intent.

Once the slot type is defined in this way, you must define the name of the slot to be used in each intent and declare its slot type. For example, you can declare a "pizzaType" slot for the pizza type information and a "pizzaAmount" slot for the number of pizzas for the "OrderPizza" intent, and designate the "PIZZA_TYPE" you defined as a custom slot type and the predefined `CLOVA.NUMBER` built-in slot type to the slots, respectively.

###  Sample utterance {#UtteranceExample}

You can list various sample of user utterances when defining the intent. The user expresses the words with same intents in various ways depending on the tone or the situation. These sample become the base data necessary for Clova to recognize diverse user expressions with similar intentions and can be used for identifying the location of the aforementioned slot within user utterance. Well written sample utterances help to build a strong interaction model for user intention recognition. When writing sample utterances, it is highly recommended that you follow the recommendations below:

* Input as many sample utterances as possible conveying the same intention but with different expressions.
* Write sample utterances with variations without duplicated patterns.
* Follow the standard below for the required number of sample utterances:
  * If a slot used in the Intent is in a built-in slot type or a custom slot type with a human-recognizable dictionary size, there must be at least 30 utterance sentences that contain the slot.
  * For a slot type in an intent with a massive dictionary, such as singer names, song titles, movie titles, or company names, there must be at least 100 utterance sentences that contain the slot.
  * For an intent with expressions in a simple form, only around 10 utterance examples need to be registered.
* If a new expression is found or there has been a recognition problem with the exiting expression, you should add new samples, even after completing the sample utterances according to the recommendations.
* If there is an ambiguous value that is difficult to determine as a slot among the values registered in the slot type dictionary, you should specify it as a slot by using it in a sample utterance. However, it is even better to not use any ambiguous values as a slot.

The image below provides an understanding on the variations needed for sample utterance without duplicated patterns.

![](/Design/Assets/Images/Extension_Design-Diagram_for_Utterance_Example.png)

For example, let's assume that the following sample utterances was written on intent (OrderPizza) for the pizza delivery service.

```
Order one box of pepperoni pizza.
Call for one box of pepperoni pizza.
Get me one box of pepperoni pizza.
I want one box of pepperoni pizza.
```

If Clova learns the above sample utterances, there is a higher possibility for an utterance that includes the values `"pepperoni"` or `"one box"` to be recognized as the `OrderPizza` intent. For example, this means that an utterance that intends to check the menu, such as "How much is one box of pepperoni pizza?" is likely to be processed as a request to order pizza.

To prevent this, we recommend that you write writing sample utterances in the following format:

```
Order two boxes of pepperoni pizza.
Could you order one box of BBQ pizza?
Please order three combinations.
Get one shrimp gold-crust pizza.
```

The above utterances are organized in the pattern of noun (pizza type), noun (quantity), and verb (intention), and it also includes commonly used prepositions, endings, adverbs, and interjections. Once you have used up all of the possible utterance patterns, you can reuse the patterns but change the slot values to meet the recommended number of utterance sentences. Add sample utterances by complying with the following details:
* Add new sentences by changing the slot value used in the sample utterance.
* Add new sentences by changing the style of the sentence such as the use of prepositions, endings, adverbs, and interjections.
* **Make sure to avoid repeated use of set combination values.** For example, the utterances "Order two boxes of pepperoni pizza." and "Please order just one pepperoni pizza for me." may have different endings, prepositions, and quantity values, but they have an overlapping combination of the values "pepperoni" and "order."

```
Quick, order 5 BBQs.
I'll just have one pepperoni.
One vegetable pizza, please.
Please get four tasty cheese pizzas for me.
```

Then the utterance related to the "OrderPizza" intent is enumerated as text and displays the slot sections as below:

{% raw %}

```
Order <pizzaAmount>two boxes</pizzaAmount> <pizzaType>of pepperoni pizza</pizzaType>.
Could you order <pizzaAmount>one box</pizzaAmount> of <pizzaType>BBQ pizza</pizzaType>?
Please order <pizzaAmount>three</pizzaAmount> <pizzaType>combinations</pizzaType>.
Get one <pizzaType>shrimp gold-crust pizza</pizzaType>.
Quick, order <pizzaAmount>5</pizzaAmount> <pizzaType>BBQs</pizzaType>.
I'll just have <pizzaAmount>one box</pizzaAmount> of <pizzaType>pepperoni</pizzaType>.
One <pizzaType>vegetable pizza</pizzaType>, please.
Please get <pizzaAmount>four</pizzaAmount> tasty <pizzaType>cheese pizzas</pizzaType> for me.
...
```
{% endraw %}

<div class="note">
  <p><strong>Note!</strong></p>
  <p>You can make a continued optimization efforts through <a href="/DevConsole/Guides/Test_Custom_Extension.md#TestInteractionModel">interaction model testing</a> or actual user logs. It is ideal for persons other than the writers of the utterances to test the interaction model. This method will be helpful to find new expression patterns.</p>
</div>


If you [register the interaction model] (/DevConsole/Guides/Register_Interaction_Model.md) as defined above using the [Clova developer console] (/DevConsole/ClovaDevConsole_Overview.md), the [registered custom extension] (/DevConsole/Guides/Register_Custom_Extension.md) will receive the following JSON message.

{% raw %}

```json
// Custom intent: Get me two pepperoni pizzas.
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
          "value": "Pepperoni"
        }
      }
    }
  }
}

// Built-in intent: Cancel, please.
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

## Deciding the sound output type {#DecideSoundOutputType}

The custom extension must send results to the user through Clova after handling the user request. Clova basically uses sound when having dialogues with the user and also uses sound when responding to the user, as well as when receiving requests. Clova provides the response types, that use sound, as shown below. Depending on the user scenario, the custom extension must provide a response using the appropriate types.

* [Speech output type](#OutputSpeech)
* [Audio content play type](#AudioPlayer)

###  Speech output type {#OutputSpeech}

This is a text-to-speech (TTS) output type that uses a processed voice to express the information input via text using speech synthesis technology. It is widely used for the purpose of answering the questions or requests of a user. A response can be composed of one or more speech output (TTS), and a short sound effect (.mp3) can also be provided simultaneously.

When writing responses as speech output type, you must conform to the following rules:
* It is recommended that one response should be less than 500 characters or 90 seconds. A user may have difficulty understanding if the response is too long.
* Use relatively short audio content for sound effects.
* The sound effect must be configured to play only once at the very beginning of the response.
  * Correct example: Sound effect (.mp3) > Speech output (TTS) > Speech output (TTS) > ...
  * Incorrect example 1: Speech output (TTS) > Sound effect (.mp3) > Speech output (TTS) > ...
  * Incorrect example 2: Sound effect (.mp3) > Speech output (TTS) > Sound effect (.mp3) > Speech output (TTS) > ...
* We recommend that a response is comprised of less than 10 speech outputs.

Following is an example of a speech output type:
```
// Example 1: Short introductory sentence
"This is a quiz on naming capital cities."

// Example 2: Response consisting of detailed explanation and quiz
No. 1 TTS: "Say 'Stop' to stop the quiz, say 'Again' to hear the question again and say 'Another Quiz' to try a different question."
No. 2 TTS: "What is the capital of United States?"

// Example 3: Response including a sound effect
No. 1 Sound effect (.mp3): Play sound effect for correct answer
No. 2 TTS: "That is correct."
```

<div class="note">
  <p><strong>Note!</strong></p>
  <p>For the information on implementing the response of speech output type, see <a href="/Develop/Guides/Build_Custom_Extension.md#ReturnCustomExtensionResponse">Returning a custom extension response</a>.</p>
</div>


### Audio content play type {#AudioPlayer}

This sound type outputs audio content such as music for a long period of time and is mainly used by the skill that provides music. This type is used to play audio content for a long time as well as to pause, resume, or stop playing. So in order for the response to be made in the audio content play type, a custom extension must have the audio content playing abilities as follows:

* A custom extension must return a response message (`AudioPlayer.Play`) upon user request for the client to play audio content.
* A custom extension must be able to pause or stop audio content anytime the user wants.
* A custom extension must be able to resume or play the paused or stopped content.
* A custom extension must be able to skip the music to the previous or next song.

Below is a simple usage scenario of audio content play type.

> <p class="ldiag">"Start Classical Box"</p>
> <p class="rdiag">"Which classical music should I turn on?" (TTS)</p>
> <p class="ldiag">"Play Vivaldi's Four Seasons."</p>
> <p class="rdiag">"Okay. Here is the first movement of Vivaldi's Four Seasons." (TTS)</p>
> <p class="rdiag">Direct to play the first movement of Vivaldi's Four Seasons (AudioPlayer.Play)</p>
> <p class="ldiag">"Clova, pause for a moment."</p>
> <p class="rdiag">Direct to pause (PlaybackController.Pause)</p>
> <p class="ldiag">"Clova, play again."</p>
> <p class="rdiag">Direct to resume (PlaybackController.Pause)</p>
> <p class="ldiag">"Next"</p>
> <p class="rdiag">Direct to play the second movement of Vivaldi's Four Seasons (AudioPlayer.Play)</p>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>For information on how to respond using an audio content play type, see <a href="/Develop/Guides/Provide_Audio_Content.md">Providing audio content</a>.</p>
</div>

## Continuous updates {#ContinuousUpdate}

During the custom extension development phase, user scenarios are created based on anticipated user utterances which are then applied to the custom extension. This helps to develop the custom extension, however, the actual way that it is used by users can be different and there can even be some unexpected use patterns from users. Basically, users can use the custom extension differently than anticipated. Therefore, continuous improvement efforts are required to enhance user satisfaction after deploying the custom extension such as efforts to improve the custom extension functions and conversation flow.

Update the custom extension after registration, by analyzing the statistics provided by the Clova platform or the incoming user utterance record (scheduled to be provided in the future).
