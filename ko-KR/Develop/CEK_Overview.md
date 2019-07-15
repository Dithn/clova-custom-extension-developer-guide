<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of shared content: CEKOverview -->

# CEK 개요
이 문서는 Clova Extensions Kit(이하 CEK)에 대해 자세히 설명합니다. 이 문서를 통해 CEK가 무엇이고 어떻게 동작하는지 파악할 수 있으며, CEK와 관련된 가이드나 레퍼런스를 제공합니다.

## CEK란? {#WhatisCEK}
CEK는 Clova extension(이하 extension)을 개발 및 배포할 때 필요한 도구와 인터페이스를 제공하는 플랫폼으로 Clova와 extension 사이의 커뮤니케이션을 지원합니다. Extension은 음악, 쇼핑, 금융 등과 같은 외부 서비스(3rd party service)나 집안의 IoT 기기 제어 등 사용자가 Clova를 통해 다양한 경험을 제공받을 수 있도록 Clova에게 확장된 기능을 제공하는 웹 애플리케이션입니다.

![](/Develop/Assets/Images/CEK_Concept_Diagram.png)

## CEK 동작 구조 {#CEKInteractionStructure}
Clova는 사용자의 발화를 인식하며, CEK를 통해 미리 [등록된 interaction 모델](/DevConsole/Guides/ManageCustomExtension/Register_Interaction_Model.md)을 참조하여 사용자의 발화를 분석합니다. CEK는 분석된 사용자의 발화 정보를 extension에게 전달하며, extension은 사용자 요청에 대한 처리 결과를 응답으로 돌려줘야 합니다. 이때 미리 정의된 메시지 포맷에 맞게 메시지를 주고 받게 됩니다. CEK와 extension의 통신은 <a href="https://tools.ietf.org/html/rfc2616" target="_blank">HTTP/1.1 프로토콜</a>을 사용해야 합니다.

다음은 Clova 플랫폼과 extension 사이의 동작 구조를 나타내는 다이어그램입니다.

![](/Develop/Assets/Images/CEK_Interaction_Structure.png)


## Extension 종류 {#ExtensionType}
Clova 플랫폼은 다음과 같은 두 종류의 extension을 지원 및 제공하고 있습니다.

* [Custom extension](/Develop/Guides/Build_Custom_Extension.md): 임의의 확장된 기능을 제공하는 extension입니다. Custom extension을 사용하면 음악, 쇼핑, 금융과 같은 외부 서비스의 기능을 제공할 수 있습니다.
* [Clova Home extension]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md): IoT 기기 제어 서비스를 제공하기 위한 extension입니다.

<!-- End of shared content -->
