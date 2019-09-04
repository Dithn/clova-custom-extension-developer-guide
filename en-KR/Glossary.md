<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: Glossary -->

# Terms and abbreviations

<div class="note">
  <p><strong>Note!</strong></p>
  <p>This page is updated on an ongoing basis.</p>
</div>

### CEK
The abbreviation of [Clova Extensions Kit](#CEK).

### CIC
The abbreviation of [Clova Interface Connect](#CIC).

### Clova {#Clova}
[Clova](https://clova.ai) is an AI (artificial intelligence) platform developed and serviced by {{ book.ServiceEnv.OrientedService }}. Clova recognizes user speech or images, analyzes them, and provides information or services that users have requested. Third-party developers, by leveraging the Clova technologies, can make a device or home appliance that provides an AI service. They can also offer their content or services to users through Clova.

### Clova developer console {#ClovaDeveloperConsole}
A <a target="_blank" href="{{ book.ServiceEnv.DeveloperConsoleURI }}">[web tool]</a> that provides the following features to the [Clova extension](#ClovaExtension) developers or to client devices that interact with the Clova platform.
* [Registering](/DevConsole/Guides/Register_Custom_Extension.md) and [deploying](/DevConsole/Guides/Deploy_Custom_Extension.md) a Clova extension
* [Registering an interaction model](/DevConsole/Guides/Register_Interaction_Model.md)
* Viewing statistics related to Clova services (coming soon)

### Clova extension {#ClovaExtension}
A web application that provides extended capabilities to Clova for a more diverse user experience, including the capability of controlling IoT appliances at home or third-party services such as music, shopping, and banking. It is commonly referred to as an "extension" and the Clova platform currently supports and provides two types of Clova extensions. To regular users, the extension is referred to as a "skill."
* [Custom extension](#CustomExtension)
* [Clova Home extension](#ClovaHomeExtension)

### Clova Extensions Kit (CEK) {#CEK}
A platform that provides tools and interfaces for development and deployment of a Clova extension. It supports [communication between Clova and extensions](/Develop/CEK_Overview.md).

### Clova Home extension {#ClovaHomeExtension}
An [extension](#ClovaExtension) that provides a service for controlling IoT appliances. For more information, see [Building a Clova Home extension]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md).

### Clova Interface Connect (CIC) {#CIC}
A platform that serves as an interface between Clova and a client aiming to provide AI assistant services such as PC/mobile apps, mobile devices, or home appliances. For more information, see [CIC overview]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/CIC_Overview.md).

### Clova app {#ClovaApp}

A Clova app developed by {{ book.ServiceEnv.OrientedService }} and deployed to the iOS or Android platform. These apps can send commands to Clova and also register and manage client devices.

### Content template {#ContentTemplate}
Standardized formats for displaying content returned from CIC. For more information, see [Content template]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/Content_Templates.md).

### Custom extension {#CustomExtension}
An [extension](#ClovaExtension) that provides extended capabilities. A custom extension lets you provide third-party services such as music, shopping, or banking. For more information, see [Building a custom extension](/Develop/Guides/Build_Custom_Extension.md).

### Custom extension messages {#CustomExtMessage}
Messages used by [Clova Extensions Kit](#CEK) and [custom extension](#CustomExtension) to exchange information. For more information, see [Custom extension messages](/Develop/References/Custom_Extension_Message.md).

### Extension {#Extension}
Another name for a [Clova extension](#ClovaExtension)

### Extension page {#ExtensionPage}

A page displayed when a specific extension is selected in the Skill Store Home (**extension service management** menu), and it provides a detailed description of the extension.

### Intent {#Intent}
An intent is a segment that distinguishes the user requests for a Clova extension to handle. The intent is divided into two: a custom intent and a built-in intent. Before implementing a [custom extension](#CustomExtension), an [interaction model](#InteractionModel) consisting of a set of intents has to be defined. For more information, see [Defining an interaction model](/Design/Design_Custom_Extension.md#DefineInteractionModel).

### IntentRequest {#IntentRequest}

A type of request message that sends the analysis details of a user request ([Intent](#Intent)) to a [custom extension](#CustomExtension). For more information, see [Handling a custom extension request](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest).

### Interaction model {#InteractionModel}
A model that defines a rule to convert to the standardized format (JSON) for [Custom extension](#CustomExtension) to pass user voice requests to an extension. For more information, see [Defining an interaction model](/Design/Design_Custom_Extension.md#DefineInteractionModel).

### LaunchRequest {#LaunchRequest}
A type of request message that notifies that a user has requested to start a certain mode or [custom extension](#CustomExtension). For more information, see [Handling a custom extension request](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest).

### OAuth 2.0
A public standard for delegation of access permission. The protocol allows internet users to grant account access right to other web services or applications. On the Clova platform, it is used for [account linking](/Develop/Guides/Link_User_Account.md) when clients try to obtain [Clova access tokens](#ClovaAccessToken) or users try to use a certain extension. For more information, go to [https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749).

### SessionEndedRequest {#SessionEndedRequest}
A type of request message that notifies that a user has requested to end a certain mode or [custom extension](#CustomExtension). For more information, see [Handling a custom extension request](/Develop/Guides/Build_Custom_Extension.md#HandleCustomExtensionRequest).

### Skill {#Skill}

An extension or service provided to users by Clova. To provide a skill to users, you must develop a [Clova extension](#ClovaExtension).

### Skill Store {#SkillStore}

A platform designed to provide skills to users.

### Skill Store home {#SkillStoreHome}

A page to display skills registered in the Skill Store. It is a term used to refer to the **extension service management** menu in the Clova app.

### Slot {#Slot}
Information necessary for processing a request declared in an [intent](#Intent). It must be defined in pair with the intent. Clova analyzes a user request and extracts information specific to the slots. For more information, see [Defining an interaction model](/Design/Design_Custom_Extension.md#DefineInteractionModel).

### Account linking {#AccountLinking}
Used when an [extension](#ClovaExtension) provides a third-party service that requires users to authenticate their account. For more information, see [Linking user account](/Develop/Guides/Link_User_Account.md).

### Sample utterance {#UserUtteranceExample}

A list of examples showing the input method of verbal user requests. You can define several example sentences for each [intent](#Intent). Example sentences are denoted with [slots](#Slot). For more information, see [Defining an interaction model](/Design/Design_Custom_Extension.md#DefineInteractionModel).

### Session ID {#SessionID}
A session identifier used by [extensions](#ClovaExtension) to distinguish context of user requests. Generally, a one-time request has a unique session ID whereas a particular mode or multi-turn request has the same session ID. A session ID is created when [Clova Extensions Kit](#CEK) sends a user request to an extension. The same session ID is maintained when requests such as [LaunchRequest](#LaunchRequest) are made or when an extension sets the `response.shouldEndSession` field to `false`. For more information, see [Building a custom extension](/Develop/Guides/Build_Custom_Extension.md).

<!-- End of the shared content -->
