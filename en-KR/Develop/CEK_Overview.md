<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: CEKOverview -->

# CEK Overview
This document provides details on Clova Extensions Kit (hereinafter referred to as "CEK"). This document will help you understand how CEK operates, and provide CEK guides and references.

## What is CEK? {#WhatisCEK}
CEK is a platform that provides tools and interfaces for developing and deploying a Clova extension (hereinafter referred to as "extension"). CEK supports communication between Clova and extensions. An extension is a web application that provides extended capabilities to Clova for a more diverse user experience, including the capability to control IoT appliances at home, or third-party services such as music, shopping, and banking services.

![](/Develop/Assets/Images/CEK_Concept_Diagram.png)

## CEK interaction structure {#CEKInteractionStructure}
Clova recognizes user utterances and analyzes them based on the [registered interaction model](/DevConsole/Guides/Register_Interaction_Model.md) in CEK. CEK delivers the analyzed user utterance information to the extension, and the extension must return the processed result of user request as a response. During this process, the exchanged messages follow the predefined message format. The communication between CEK and the extension must use the <a href="https://tools.ietf.org/html/rfc2616" target="_blank">HTTP/1.1 protocol</a>.

The diagram below shows the operating structure between the Clova platform and extension.

![](/Develop/Assets/Images/CEK_Interaction_Structure.png)


## Extension types {#ExtensionType}
The Clova platform currently supports and provides two types of extensions as follows:

* [Custom extension](/Develop/Guides/Build_Custom_Extension.md): An extension that provides tailored extended functions. A custom extension lets you provide third-party services such as music, shopping, or banking.
* [Clova Home extension]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md): An extension that provides a service for controlling IoT appliances.

<!-- End of the shared content -->
