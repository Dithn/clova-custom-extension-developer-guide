<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: CEKOverview -->

# CEKの概要
このドキュメントでは、Clova Extensions Kit（以下、CEK）について説明します。Extensionの開発者は、このドキュメントから、CEKとは何で、どのような仕組みで動作するかを理解できます。また、ここではCEKに関するガイドとリファレンスも提供されます。

## CEKとは？ {#WhatisCEK}
CEKは、Clova Extension（以下、Extension）を開発および配布する際に必要なツールとインターフェースを提供するプラットフォームです。ClovaとExtensionのコミュニケーションをサポートします。Extensionは、音楽、ショッピング、金融などの外部のサービス（サードパーティサービス）、または家庭のIoTデバイスの制御など、Clovaの機能を拡張して、ユーザーに様々な経験を提供するWebアプリケーションです。

![](/Develop/Assets/Images/CEK_Concept_Diagram.png)

## CEKの仕組み {#CEKInteractionStructure}
Clovaは、ユーザーの発話を認識すると、CEKを介し、あらかじめ[登録された対話モデル](/DevConsole/Guides/Register_Interaction_Model.md)を参照してユーザーの発話を解析します。CEKは解析されたユーザーの発話をExtensionに渡し、Extensionはユーザーリクエストの処理結果をレスポンスとして返す必要があります。その際、すでに定義されたメッセージフォーマットに合わせてメッセージのやり取りをします。CEKとExtensionのコミュニケーションは<a href="https://tools.ietf.org/html/rfc2616" target="_blank">HTTP/1.1プロトコル</a>を使用します。

ClovaプラットフォームとExtensionは、次のように情報をやり取りします。

![](/Develop/Assets/Images/CEK_Interaction_Structure.png)


## Extensionの種類 {#ExtensionType}
Clovaプラットフォームは、次の2種類のExtensionをサポートおよび提供しています。

* [Custom Extension](/Develop/Guides/Build_Custom_Extension.md)：任意に拡張された機能を提供するExtensionです。Custom Extensionを利用すると、音楽、ショッピング、金融など、外部サービスの機能を提供できます。
* [Clova Home Extension]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md)：IoTデバイス制御サービスを提供するExtensionです。

<!-- End of the shared content -->
