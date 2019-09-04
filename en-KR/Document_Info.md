# README
This document provides design guidelines, developer guide/API references for CEK platforms and a guide for Clova developer console in order to help develop a Clova custom extension. The intended readers of this document are custom extension developers using CEK to provide online content and services.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Clova is under continuous development. Thus, the information in this document is subject to change at any time.</p>
</div>

## Contacts
For any inquiries about this document, contact the Clova partnership team or put your query in our <a href="{{ book.ServiceEnv.DeveloperCenterForumURI }}" target="_blank">{{ book.ServiceEnv.DeveloperCenterName }}forum</a>.

## Document revision history

The revision history of this document is as follows:

<table>
  <thead>
    <tr>
      <th style="width:12%">Release date</th><th style="width:88%">History</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-08-19</td>
      <td>
        <ul>
          <li>Moved up a level of the table of contents in the API reference and the developer console guide.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-08-05</td>
      <td>
        <ul>
          <li>Separated the Design guidelines for Custom extensions document into <a href="/Design/Design_Custom_Extension.md">Designing the custom extension</a>, <a href="/Design/Supported_Audio_Format.md">Supported audio formats</a>, and <a href="/Design/Rules_For_Content.md">Guidelines for providing content</a> pages</li>
          <li>Added the container formats supported by Clova in the details of <a href="/Design/Audio.md#SupportedAudioFormat">Supported audio formats</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-31</td>
      <td>
        <ul>
          <li>Separated into Clova custom extension guide from Clova developer guide.</li>
          <li>Corrected some link errors and revised misprints</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-02</td>
      <td>
        <ul>
          <li>Added restrictions related to registering sample utterances and uploading TSV files in <a href="/DevConsole/Guides/Register_Interaction_Model.md">Registering an interaction model</a></li>
          <li>Revised some misprints of examples and adjusted the level of note box</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-06-10</td>
      <td>
        <ul>
          <li>Added Clova.RequestAlternativesIntent to the <a href="/Design/Design_Custom_Extension.md#BuiltinIntent">Built-in intent</a> in the <a href="/Design/Design_Custom_Extension.md">Designing custom extensions</a> for new content playback.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-20</td>
      <td>
        <ul>
          <li>Token and URL fields of <a href="/Develop/References/CEK_API.md#CustomExtSpeechInfoObject">SpeechInfoObject</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-04-01</td>
      <td>
        <ul>
          <li>Added a token field to the <a href="/Develop/References/CEK_API.md#CustomExtSpeechInfoObject">SpeechInfoObject</a> and the <a href="/Develop/Guides/Monitor_TTS_Playback_Status.md">Checking TTS playback status</a> guide so that the extension can receive reports on the speech (TTS) playback status of the client</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-03-25</td>
      <td>
        <ul>
          <li>Added the contentType field to the <a href="/Develop/References/CEK_API.md#CustomExtSpeechInfoObject">SpeechInfoObject</a> of the custom extension message to provide the HLS audio</li>
          <li>Corrected some errors and examples in the link</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-03-13</td>
      <td>
        <ul>
          <li>Modified the order of the details of some reference documents</li>
          <li>Reflected internal and external feedback on the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-02-26</td>
      <td>
        <ul>
          <li>Clarified the term extension, when used without descriptions in the document, because it may refer to either a custom extension or a Clova Home extension</li>
          <li>Revised the term URL to URI for those without the UI components</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-25</td>
      <td>
        <ul>
          <li>Revised some styles of the diagram and adjusted the table column interval</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-07</td>
      <td>
        <ul>
          <li>Unified the style of some UML diagrams used in the document</li>
          <li>Corrected some notation errors in the document history</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-12-24</td>
      <td>
        <ul>
          <li>Corrected the misprinted name of the directive message in the example scenario in the audio content playback type description in <a href="/Design/Design_Custom_Extension.md">Designing the custom extension</a></li>
          <li>Revised the expression for introducing the CIC specification so that the readers can easily recognize such introduction in <a href="/Develop/Guides/Provide_Audio_Content.md">Providing audio content</a></li>
          <li>Corrected the notation error in the note type of some sequence diagrams</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-12-22</td>
      <td>
        <ul>
          <li>Removed the incorrectly stated vendorId field from redirect_uri parameters in the <a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">Building an authorization server</a> description of the <a href="/Develop/Guides/Link_User_Account.md">Linking user accounts</a> guide</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-10-13</td>
      <td>
        <ul>
          <li>Specified that at least one and up to three invocation names can be registered when <a href="/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo">entering basic extension information</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-21</td>
      <td>
        <ul>
          <li>Added the description of the SignatureCEK field in the <a href="/Develop/References/HTTP_Message.md#HTTPHeader">HTTP header</a> to verify the messages sent from Clova, and added a section on verifying each request message to <a href="/Develop/Guides/Build_Custom_Extension.md">Building a custom extension</a></li>
          <li>Corrected some wrong code examples</li>
          <li>Corrected some incorrect links</li>
          <li>Changed some notations of the word Extension that is at the point of interaction with the end user, to Skill (Also updated the UI capture images)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-07</td>
      <td>
        <ul>
          <li>Added recommendations on attributes and loudness for sound quality by audio content type in the <a href="/Design/Supported_Audio_Format.md">Supported audio formats</a> section of Design guidelines</li>
          <li>Changed "yourdomain.com" used in examples to "example.com," which is the domain name for document preparation</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-24</td>
      <td>
        <ul>
          <li>Corrected an error in the example of an <a href="/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest">EventRequest</a> type of a <a href="/Develop/References/Custom_Extension_Message.md">custom extension</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-09</td>
      <td>
        <ul>
          <li>Corrected typos in the section on <a href="/Develop/Guides/Build_Custom_Extension.md#ProvidingMetaDataForDisplay">Providing audio content metadata for display</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-07-09</td>
      <td>
        <ul>
          <li>Added a guideline for <a href="/Design/Design_Custom_Extension.md#DefineInvocationName">defining the name</a> of the Custom extension</li>
          <li>Added a guideline for <a href="/Design/Rules_For_Content.md">Guidelines for providing content</a> on the Custom extension</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-06-25</td>
      <td>
        <ul>
          <li>Added a guideline for the <a href="/Design/Design_Custom_Extension.md#DecideSoundOutputType">response types</a> of the Custom extension</li>
          <li>Added a section on <a href="/Develop/Guides/Provide_Audio_Content.md">Providing audio content</a> in Building a custom extension</li>
          <li>Added the <a href="/Develop/References/Custom_Extension_Message.md#CustomExtEventRequest">EventRequest type</a> to the <a href="/Develop/References/Custom_Extension_Message.md#CustomExtRequestType">request types</a> of custom extension messages</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-14</td>
      <td>
        <ul>
          <li>Added HTTP request message headers (SignatureCEK, SignatureCEKCertChainUrl) and a section on Validating a request message</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-02</td>
      <td>
        <ul>
          <li>Reflected some UI updates on the Clova developer console</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-05</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/Tutorials/Use_Builtin_Type_Slots.md">Utilizing user-input information</a> page in the <a href="/Develop/Tutorials/Introduction.md">tutorial</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-05</td>
      <td>
        <ul>
          <li>Modified the description of invoking the extension (<a href="CEK/Guides/Build_Custom_Extension.md#HandleLaunchRequest">LaunchRequest</a>) and applied the changed contents to the <a href="/Design/Design_Custom_Extension.md">Designing custom extensions</a></li>
          <li>Specified <a href="/Develop/CEK_Overview.md#WhatisCEK">HTTP versions</a> used for communicating between CEK and extensions</li>
          <li>Added the <a href="/Develop/Tutorials/Handle_Builtin_Intents.md">Handling built-in intents</a> page to the <a href="/Develop/Tutorials/Introduction.md">tutorial</a></li>
          <li>Specified the <a href="/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection">port</a> to be used by the extension server</li>
          <li>Corrected some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-29</td>
      <td>
        <ul>
          <li>Added the method to check the connection before <a href="/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection">setting up an extension server connection</a> and added a guide on the <a href="/DevConsole/Guides/Test_Custom_Extension.md#TestOnClovaApp">automated application of tester IDs</a></li>
          <li>Reflected some UI updates on the Clova developer console</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-22</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Supported_Audio_Format.md">Supported audio formats</a> to the Design guidelines</li>
          <li>Added the <a href="/Develop/Tutorials/Introduction.md">Tutorial</a> page and the <a href="/Develop/Tutorials/Build_Simple_Extension.md">Building a basic extension</a> page</li>
          <li>Displayed the <a href="/DevConsole/Guides/Register_Interaction_Model.md#AddCustomSlotType">list of built-in intents</a> and added UI for writing messages for a <a href="/DevConsole/Guides/Deploy_Custom_Extension.md#InputComplianceInfo">review request</a></li>
          <li>Changed the image format of the UML diagram</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-08</td>
      <td>
        <ul>
          <li>Modified the description of the <a href="/Design/Design_Custom_Extension.md#DefineInteractionModel">built-in intent</a> based on platform implementation</li>
          <li>Added the <a href="/Develop/Examples/Extension_Examples.md">Extension examples</a> page</li>
          <li>Updated the description on <a href="/DevConsole/Guides/Test_Custom_Extension.md">testing an extension</a> due to the added <strong>tester ID</strong> field</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-02</td>
      <td>
        <ul>
          <li>Corrected some errors and misprints in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-18</td>
      <td>
        <ul>
          <li>Moved the section on <a href="/Design/Design_Custom_Extension.md#DefineInteractionModel">Defining an interaction model</a> in <a href="/DevConsole/Guides/Register_Interaction_Model.md">Registering interaction model</a> to <a href="/Design/Design_Custom_Extension.md">Designing custom extensions</a></li>
          <li>Added a guideline to prepare <a href="/Design/Design_Custom_Extension.md#UtteranceExample">sample utterances</a> in the section on <a href="/Design/Design_Custom_Extension.md#DefineInteractionModel">Defining an interaction model</a></li>
          <li>Added a description on how to use the test mode in <a href="/DevConsole/Guides/Test_Custom_Extension.md">Testing an extension</a></li>
          <li>Modified images and descriptions following UI improvements</li>
          <li>Added sections <a href="/DevConsole/Guides/Update_Custom_Extension.md">Updating an extension</a> and <a href="/DevConsole/Guides/Remove_Custom_Extension.md">Disabling and deleting an extension</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-11</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Design_Custom_Extension.md">Designing custom extensions</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-04</td>
      <td>
        <ul>
          <li>Added a reprompt field in the <a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">response message</a> for multi-turn dialogues with the user</li>
          <li>Corrected some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-06</td>
      <td>
        <ul>
          <li>Changed the name of the context.System.device.displayType field to context.System.device.display from the request messages in <a href="/Develop/References/Custom_Extension_Message.md">custom extension messages</a> and changed the sub-field configuration</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-30</td>
      <td>
        <ul>
          <li>Added the description in the <a href="/DevConsole/ClovaDevConsole_Overview.md">Clova developer console overview</a></li>
          <li>Added the guide for <a href="/DevConsole/Guides/Register_Custom_Extension.md">Registering an extension</a></li>
          <li>Added the guide for <a href="/DevConsole/Guides/Register_Interaction_Model.md">Registering an interaction model</a></li>
          <li>Added the guide for <a href="/DevConsole/Guides/Deploy_Custom_Extension.md">Deploying an extension</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-23</td>
      <td>
        <ul>
          <li>Added context.System.device.displayType field to the request message in <a href="/Develop/References/Custom_Extension_Message.md">custom extension messages</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-16</td>
      <td>
        <ul>
          <li>Revised some images and corrected errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-04</td>
      <td>
        <ul>
          <li>Corrected some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-14</td>
      <td>
        <ul>
          <li>Added the section on <a href="/Develop/Guides/Do_Multiturn_Dialog.md">Engaging in multi-turn dialogues</a> and updated the description of the sessionAttributes field</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-07</td>
      <td>
        <ul>
          <li>Updated the <a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">outputSpeech</a> object configuration in the <a href="/Develop/References/Custom_Extension_Message.md#CustomExtResponseMessage">response message of custom extensions</a></li>
          <li>Added the <a href="/Glossary.md">Glossary</a></li>
          <li>Updated the table of contents for the section on CEK message formats</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-03</td>
      <td>
        <ul>
          <li>Updated images in the CEK document</li>
          <li>Applied changes according to the document review results</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-19</td>
      <td>
        <ul>
          <li>Completed the CEK part of the document</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
