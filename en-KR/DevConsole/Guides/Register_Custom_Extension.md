<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Registering a custom extension
If you are developing or have developed a [Custom extension](/Develop/Guides/Build_Custom_Extension.md), you must register it on the [Clova developer console](/DevConsole/ClovaDevConsole_Overview.md). To register a new custom extension, click **{{ book.DevConsole.cek_new_skill }}** at the bottom of the CEK menu page.

![](/DevConsole/Assets/Images/DevConsole-First_Look_of_Extension_List.png)

To register a custom extension, you must typically complete the following steps in order:

1. [Agreeing to terms and conditions and privacy policy](#AgreeTermsOfUse)
2. [Entering basic custom extension information](#InputExtensionInfo)
3. [Setting a server connection](#SetServerConnection)
  * [Setting up account linking](#SetAccountLinking)

<!-- Start of the shared content: AgreeTermsOfUse -->

## Agreeing to the terms and conditions and privacy policy {#AgreeTermsOfUse}

In order to register a custom extension, you must first agree to the terms and conditions of the CEK API service and the privacy policy. The details on the terms and conditions and privacy policy will be displayed **only once** and will not be displayed after you agree.

![](/DevConsole/Assets/Images/DevConsole-Agree_Terms_of_Use_and_Collecting_Personal_Info.png)

<!-- End of the shared content -->

## Enter basic custom extension information {#InputExtensionInfo}

The first thing to do when registering a custom extension is to enter the basic custom extension information. The basic custom extension information is the essential information for creating a custom extension on the Clova developer console. After you enter the basic custom extension information, you can access or edit the created custom extension on the CEK menu at any time.

Follow the steps below to register the custom extension:

![](/DevConsole/Assets/Images/DevConsole-Create_New_Custom_Extension.png)

1. Select the type of custom extension to register from the **{{ book.DevConsole.cek_type }}** item.<br />
  Once you select a custom extension type, an input field corresponding to the type will be displayed.
2. Select a language supported from the **{{ book.DevConsole.cek_lang }}** item. Currently, only **{{ book.DevConsole.supported_languages }}** is supported.
3. Enter **extension ID**, **skill name**, **invocation name**, and **creator**.
  1. **{{ book.DevConsole.cek_id }}**<br />
    Enter the unique ID of the custom extension. . Use the reverse domain name notation for the ID (e.g. com.example.extension.pizzabot).
  2. **{{ book.DevConsole.cek_name }}**<br />
    Enter the name of the custom extension. This will be shown later on in **{{ book.DevConsole.ManageCustomExtensions }}**.
  3. **{{ book.DevConsole.cek_invocation_name }}**<br />
    Enter the name to use when the user invokes the custom extension. You can register at least one and up to three {{ book.DevConsole.cek_invocation_name }}. The name can be a general name of an owned service, company, or organization, but it is more preferable to use a concise and unique name to increase user convenience. A universal word, or names of another company or its service, cannot be used. The **{{ book.DevConsole.cek_invocation_name }}** will be evaluated during the custom extension review process.
  4. **{{ book.DevConsole.cek_provider }}**<br />
    Enter the name or alias of the custom extension creator (company or individual). This will be shown later on in the **{{ book.DevConsole.ManageCustomExtensions }}**, and will be reviewed in the custom extension approval process.
4. (If the extension uses the [AudioPlayer]({{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.{{ book.DocMeta.FileExtensionForExternalLink }}) directive message), select **{{ book.DevConsole.cek_yes }}** for the **{{ book.DevConsole.cek_audioplayer }}** item. The custom extension uses the directive messages when it provides a music streaming service.
5. Enter an email address for contact in the **{{ book.DevConsole.cek_email }}** item.
6. Enter the {{ book.ServiceEnv.OrientedService }} account for testing the custom extension under development in the **{{ book.DevConsole.cek_tester }}** item.<br />
  You do not need to enter the account information when registering the extension, but can enter it in this field when [testing the extension](/DevConsole/Guides/Test_Custom_Extension.md) later on.
7. After filling out the basic custom extension information, click **{{ book.DevConsole.cek_create }}**.

Once you complete entering the basic custom extension information, you will be directed to the screen to edit this information. From this point, you can click **{{ book.DevConsole.cek_save }}** at the bottom of the page anytime to save the changed details. You can also find the list of registered custom extensions in the CEK menu as shown below.

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_List_After_Creation.png)

## Setting up a server connection {#SetServerConnection}

The custom extension will make an HTTPS connection to CEK. Here, CEK sends an HTTP request to the custom extension and the custom extension sends an HTTP response to CEK. In order for CEK to send an HTTP request to the custom extension, you must set up a server connection from the Clova developer console. You can set up a server connection after you [enter the basic custom extension information](#InputExtensionInfo) for the created extension.

Make sure to check whether a connection with the custom extension server is available before registering the custom extension server. You can check the connection state using a simple curl command as shown in the example below.

{% raw %}
```bash
$ curl "https://example.com/pizzabot" -X POST
```
{% endraw %}

Follow the steps below to set up a connection with the server.

![](/DevConsole/Assets/Images/DevConsole-Custom_Extension_Server_Settings.png)

1. Click on the **{{ book.DevConsole.cek_configuration }}** tab above the extension information input UI.
2. Enter the endpoint URI of the custom extension server in the **{{ book.DevConsole.cek_service_endpoint_url }}** item.
  <div class="note">
    <p>**Note!**</p>
    <p>An HTTP connection can be used for testing but an HTTPS connection is required for the official service. The custom extension server must use ports 80 and 443 for HTTP and HTTPS connections, respectively.</p>
  </div>
3. (If [account linking](#SetAccountLinking) is required), select **{{ book.DevConsole.cek_yes }}** for the **{{ book.DevConsole.cek_account_linking }}** item.
4. Click the radio button of the **{{ book.DevConsole.cek_ssl_certificate }}** item. The custom extension server must use the certificate of an authorized certificate agency. (Self-signed certificates cannot be used.)
5. Fill out the details for setting up the server connection and click **{{ book.DevConsole.cek_save }}**.</li>

### Setting up account linking {#SetAccountLinking}

If a link between the user account of the custom extension service and the Clova user account is required, you must enter the [account linking](/Develop/Guides/Link_User_Account.md) information in [Setting up a server connection](#SetServerConnection).

Follow the steps below to enter the [information required](/Develop/Guides/Link_User_Account.md#RegisterAccountLinkingInfo) to set up account linking.

<img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_1.png" />

1. Select **{{ book.DevConsole.cek_yes }}** in the **{{ book.DevConsole.cek_account_linking }}** item.
2. In the **{{ book.DevConsole.cek_authorization_url }}** item, enter the authorization URI where users can verify their account.<br />
  The user will be directed to this URL when the custom extension is activated.
3. If you want to allow a user to set up one’s own account right away, enter the URI for the account setup page in the **{{ book.DevConsole.cek_configuration_url }}** item.
4. Enter the **{{ book.DevConsole.cek_client_id }}** required for an HTTP request when authenticating the user account.<br />
  The client ID is the value created when [building an authentication server](/Develop/Guides/Link_User_Account.md#BuildAuthServer).
5. In the **{{ book.DevConsole.cek_privacy_policy_url }}** item, enter the URI where the privacy policy on the custom extension service is provided.<br />
  The details on this page will be shown later on in the **{{ book.DevConsole.ManageCustomExtensions }}**.
6. (If the page registered in the **{{ book.DevConsole.cek_authorization_url }}** or **{{ book.DevConsole.cek_privacy_policy_url }}** imports resources from another domain) Add the corresponding domain address in the **{{ book.DevConsole.cek_domain_list }}** item.
7. (Optional) If the usage scope of the access token, issued when linking the user account, is predefined, add the predefined scope in the **{{ book.DevConsole.cek_scope }}** item.<br />
  <img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_2.png" />
8. In the **{{ book.DevConsole.cek_access_token_uri }}** item, enter the URI to get the service access token.<br />
  Currently, **only the code grant method is supported for grant type**.
9. In the **{{ book.DevConsole.cek_refresh_token_uri }}** item, enter the URI to renew the service access token in.
10. After acquiring the service access token, enter the **{{ book.DevConsole.cek_client_secret }}** required for HTTP requests.<br />
  The client secret is the value created when [building an authentication server](/Develop/Guides/Link_User_Account.md#BuildAuthServer).
11. For **{{ book.DevConsole.cek_client_authentication_scheme }}**, set the value for implementing the authentication server interface.
  * **HTTP basic (Recommended)**: Select when the credentials are sent in the HTTP header data to acquire the service access token.
  * **Credentials in request body**: Select when the credentials are sent in the HTTP body data to acquire the service access token.

<div id="RedirectURI" class="note">
  <p><strong>Note!</strong></p>
  <p>The client URI (redirect URI) to be redirected after account authentication is <code>{{ book.ServiceEnv.RedirectURIforAccountLinking }}</code> and it can be found in <strong>{{ book.DevConsole.cek_redirect_urls }}</strong>.</strong> The <a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">authorization server</a> must verify whether this value is identical to the <code>redirect_uri</code> value sent by the client.</p>
  <img src="/DevConsole/Assets/Images/DevConsole-Redirect_URI_for_Extension_Accoun_Linking.png" />
</div>
