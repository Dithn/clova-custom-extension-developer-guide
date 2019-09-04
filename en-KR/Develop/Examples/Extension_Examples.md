# Extension examples

Here is an explanation of some extensions that are currently provided by Clova. As these extensions perform simple actions, these examples will be helpful when implementing your own extension.

* [Magic Ball](#MagicBall)
* [Rain Sound](#RainSound)
* [Dice Drawer](#DiceDrawer)
* [Coin Helper](#CoinHelper)

## Magic Ball {#MagicBall}

Magic Ball is an extension that returns a random response to a user's question, from 20 predefined positive or negative expressions.

### Features
* It has a simple interaction model, since a random response is given regardless of the user utterance.
* It can be seen as the simplest "echo" level example in terms of server-client programming.
* It is implemented in the Go programming language.

### GitHub repository
{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-magicball

## Rain Sound {#RainSound}

Rain Sound is an extension that responds by making the client play the pre-recorded audio file (.mp3) upon user request.

### Features
* The user can select the number of repeats for the rain audio. The interaction of the extension defines the value of repeats as a slot.
* It sends the [response message](/Develop/References/Custom_Extension_Message.md#CustomExtRequestType) by including not only the guide message but also the [`AudioPlayer.Play`]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message to CEK so that the client can play the audio content.
* It is implemented in Node.js.

### GitHub repository
{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-rainsound

## Dice Drawer {#DiceDrawer}

Dice Drawer is an extension that rolls a selected number of dice upon user request and tells the results. It tells the sum of results if more than one die is rolled.

### Features
* The user can select the number of dice to roll. The interaction model of the extension defines the value of the number of dice as a slot.
* The expression returned as a response varies depending on whether the number of rolled dice is one or more than one.
* It is implemented in Node.js.

### GitHub repository
{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-dice

## Coin Helper {#CoinHelper}

Coin Helper is an extension that returns the market price of cryptocurrencies. Upon user request, the extension calls the REST API provided by a cryptocurrency exchange and retrieves the market price information.

### Features
* The user can decide which cryptocurrency exchange information to use and which cryptocurrency market price to check. The interaction model of the extension defines the value from the cryptocurrency exchange and cryptocurrency as a slot.
* It uses REST API of external services to retrieve data from those services.
* It has a slightly more complicated interaction model than other examples.
* It is implemented in the Go programming language.

### GitHub repository

{{ book.ServiceEnv.GitHubBaseURIforExtensionExample }}/clova-extension-sample-coinhelper
