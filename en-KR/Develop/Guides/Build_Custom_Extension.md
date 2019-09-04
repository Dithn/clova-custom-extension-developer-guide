# Creating a custom extension

A custom extension is not a default function or service of Clova; it is a tailored extension developed by a developer or an extension providing a third-party service. For example, a custom extension not only provides services such as web search or news clipping, but also third-party services such as music, shopping, or financial services that require user account authentication. Custom extensions receive analyzed information of user utterances from CEK. It must also handle the corresponding details and return the results of handling the service.

This section explains the prerequisites for creating a custom extension, the types of messages exchanged with CEK, and the operation method in the following order:

* [Prerequisites](#Preparation)
* [Handling a custom extension request](#HandleCustomExtensionRequest)
   * [Handling a `LaunchRequest`](#HandleLaunchRequest)
   * [Handling a `IntentRequest`](#HandleIntentRequest)
   * [Handling a `SessionEndedRequest`](#HandleSessionEndedRequest)
   * [Validating a request message](#RequestMessageValidation)
* [Returning a custom extension response](#ReturnCustomExtensionResponse)

{% include "/Develop/Guides/BuildCustomExtension/Preparation.md" %}

{% include "/Develop/Guides/BuildCustomExtension/Handle_Custom_Extension_Request.md" %}

{% include "/Develop/Guides/BuildCustomExtension/Validating_Request_Message.md" %}

{% include "/Develop/Guides/BuildCustomExtension/Return_Custom_Extension_Response.md" %}
