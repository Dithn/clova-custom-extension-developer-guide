# Guidelines for providing content

There are guidelines that must be observed when providing content to users via skills (extensions). A Clova administrator reviews for any guideline violations before [publishing the skill](/DevConsole/Guides/Deploy_Custom_Extension.md) to the Skill Store. If a skill is in violation of the guidelines or deemed acceptable, the publishing request may be rejected or an already published skill can be revoked. Therefore, you must make sure that your skill adheres to the following guidelines before [requesting a review](/DevConsole/Guides/Deploy_Custom_Extension.md#RequestExtensionSubmission).

* [Completeness of skill](#SkillCompleteness)
* [Security of skill](#SkillSecurity)
* [Protection of rights and legal compliances](#RightAndLegal)
* [Morals](#Morals)
* [Privacy](#Privacy)
* [Other precautions](#OtherPrecautions)

## Completeness of skill {#SkillCompleteness}

Skills must comply with the following in order to meet the completeness guidelines and provide a better user experience by providing convenience or resolving inconveniences:

* Except under special situations, such as server maintenance, your skill must always be ready to respond to user requests.
* Information on the skill including [basic information](/DevConsole/Guides/Register_Custom_Extension.md#InputExtensionInfo), [server settings](/DevConsole/Guides/Register_Custom_Extension.md#SetServerConnection), or [deployment information](/DevConsole/Guides/Deploy_Custom_Extension.md#InputDeploymentInfo) must be updated as the latest information at all times without wrong or missing information.
* The [user scenario](/Design/Design_Custom_Extension.md#MakeUseCaseScenarioScript) must be achievable and natural. The [interaction model](/Design/Design_Custom_Extension.md#DefineInteractionModel) must be well defined and implemented for the satisfactory recognition of user requests.
* If server linking or account linking is required to provide content, your skill must meet the necessary [security conditions](/Develop/Guides/Link_User_Account.md#ApplyAccountLinking).

## Security of skill {#SkillSecurity}

Skills must comply with the following guidelines to guarantee user safety:

* Your skill must not induce or encourage any actions that may endanger human life or compromise physical safety.
* Your skill must not induce or encourage any actions that could raise safety concerns for underage persons such as running away or rebelling.
* Your skill must not interfere with the use of {{ book.DocMeta.DocOwner }} or third-party devices, equipment, or systems, or cause operational disruptions.
* Your skill must not illegally change or remove the information accumulated in the equipment of {{ book.DocMeta.DocOwner }} or a third party.
* Your skill must not contain or send harmful programs such as a virus.

## Protection of rights and legal compliances {#RightAndLegal}

Skills must comply with the following to protect rights and obligate to respect relevant laws:

* Your skill must not infringe on the rights of {{ book.DocMeta.DocOwner }} or a third party (copyrights, intellectual property rights, portrait rights, naming rights, human rights, reputation rights, or other rights).
* Your skill must not provide content with unclear rights (e.g., Secondary creation).
* Your skill must not provide content without permission or without proof of receiving permission from the rights holder.
* Your skill must not provide content that induces or encourages illegal behavior such as gambling.
* Your skill must not provide content on sexual assaults, explicit expressions of sexual activity, child pornography, or child abuse, as well as any other brutal or obscene content, including any content with it.
* Your skill must not provide content that may encourage or aid in crime or any content that violates or is in danger of violating laws or good morals.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The Clova administrator can request relevant documents to verify the content rights for the review.</p>
</div>

## Ethics {#Morals}

Skills must comply with the following ethical guidelines:

* Your skill must not cause users to scrutinize, curse, attack, or create negative feelings against a specific individual, group, corporation, or country.
* Your skill must not cause users to attack a specific religion, culture, ethnicity, or national character, or create a negative feeling against it.
* Your skill must not include antisocial content or cause discomfort.
* Your skill must clearly indicate that it is not developed or provided by {{ book.DocMeta.DocOwner }}.
* Your skill must not unjustly discriminate against or criticize {{ book.DocMeta.DocOwner }} or a third party or induce or encourage such actions from users.

## Privacy {#Privacy}

The skills must comply with the privacy responsibilities by following the guidelines below:

* Your skill must not collect any personal information such as Clova usage data.
* Your skill must not collect the following sensitive information:
  * Information about race, religion, social status, military history, crime history, or damages from crime
  * Information about handicaps, intellectual handicaps, or mental handicaps
  * Results of health diagnoses or checkups
  * Information about medical records, treatments, or drug administration
  * Information related to civil or criminal cases, such as confinement or search, as a suspect or defendant

## Other precautions {#OtherPrecautions}

The following precautions apply when providing skill content:

* If the skill is linked to your device, Clova device, or a third-party device (e.g., an IoT device), the Clova administrator may ask for the device to be submitted for review.
* Your skill must not be in violation of the [Clova Extensions Kit Terms and Conditions]({{ book.ServiceEnv.CEKTermsOfUseURI }}) along with the details mentioned above.

<div class="note">
<p><strong>Note!</strong></p>
<p>Assessing the feasibility of your skill may be difficult due to some exceptions. If so, state your opinion when you <a href="/DevConsole/Guides/Deploy_Custom_Extension.md#RequestExtensionSubmission">request a review</a>.</p>
</div>
