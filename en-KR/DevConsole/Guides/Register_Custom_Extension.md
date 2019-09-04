<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

# Registering a custom extension
If you are developing or have developed a [Custom extension](/Develop/Guides/Build_Custom_Extension.md), you must register it on the [Clova developer console](/DevConsole/ClovaDevConsole_Overview.md). To register a new custom extension, click **{{ book.DevConsole.cek_new_skill }}** at the bottom of the CEK menu page.

![](/DevConsole/Assets/Images/DevConsole-First_Look_of_Extension_List.png)

To register a custom extension, you must typically complete the following steps in order:

<ol>
  <li><a href="#AgreeTermsOfUse">Agree to the terms and conditions and privacy policy</a></li>
  <li><a href="#InputExtensionInfo">Enter basic custom extension information</a></li>
  <li><a href="#SetServerConnection">Set up server connection</a>
    <ul>
      <li><a href="#SetAccountLinking">Set up account linking</a></li>
    </ul>
  </li>
</ol>

<!-- Start of the shared content: AgreeTermsOfUse -->

## Agreeing to the terms and conditions and privacy policy {#AgreeTermsOfUse}

In order to register a custom extension, you must first agree to the terms and conditions of the CEK API service and the privacy policy. The details on the terms and conditions and privacy policy will be displayed **only once** and will not be displayed after you agree.

![](/DevConsole/Assets/Images/DevConsole-Agree_Terms_of_Use_and_Collecting_Personal_Info.png)

<!-- End of the shared content -->

## Enter basic custom extension information {#InputExtensionInfo}

The first thing to do when registering a custom extension is to enter the basic custom extension information. The basic custom extension information is the essential information for creating a custom extension on the Clova developer console. After you enter the basic custom extension information, you can access or edit the created custom extension on the CEK menu at any time.

Follow the steps below to register the custom extension:

![](/DevConsole/Assets/Images/DevConsole-Create_New_Custom_Extension.png)

<ol>
  <li>Select the type of custom extension to register from the <strong>{{ book.DevConsole.cek_type }}</strong> item. Once you select a custom extension type, an input field corresponding to the type will be displayed.</li>
  <li>Select a language supported from the <strong>{{ book.DevConsole.cek_lang }}</strong> item. Currently, only <strong>{{ book.DevConsole.ko_KR }}</strong> is supported.</li>
  <li>Enter <strong>extension ID</strong>, <strong>skill name</strong>, and <strong>invocation name</strong> in the following fields:
    <ol>
      <li><strong>{{ book.DevConsole.cek_id }}</strong>: Unique ID of the custom extension. Use the reverse domain name notation for the ID (e.g. com.example.extension.pizzabot).</li>
      <li><strong>{{ book.DevConsole.cek_name }}</strong>: Name of the custom extension. This will be shown later on <strong>{{ book.DevConsole.ManageCustomExtensions }}</strong>.</li>
      <li><strong>{{ book.DevConsole.cek_invocation_name }}</strong>: Name of the extension that can be used by users to call the custom extension. You can register at least one and up to three {{ book.DevConsole.cek_invocation_name }}. The name can be a general name of an owned service, company, or organization, but it is more preferable to use a concise and unique name to increase user convenience. A universal word, or names of another company or its service, cannot be used. The <strong>{{ book.DevConsole.cek_invocation_name }}</strong> will be evaluated during the custom extension review process.</li>
      <li><strong>{{ book.DevConsole.cek_provider }}</strong>: Enter the name or alias of the custom extension manufacturer (company or individual). This will be shown later on the <strong>{{ book.DevConsole.ManageCustomExtensions }}</strong>, and will be reviewed in the custom extension approval process.</li>
    </ol>
  </li>
  <li>If the extension uses the <a href="{{ book.DocMeta.ClovaClientDeveloperGuideBaseURI }}/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> directive message, select <strong>{{ book.DevConsole.cek_yes }}</strong> from the <strong>{{ book.DevConsole.cek_audioplayer }}</strong> item. The custom extension uses the directive messages when it provides a music streaming service.</li>
  <li>Enter an email address for contact in the <strong>{{ book.DevConsole.cek_email }}</strong> item.</li>
  <li>Enter the {{ book.ServiceEnv.OrientedService }} account for testing the custom extension under development in the <strong>{{ book.DevConsole.cek_tester }}</strong> item. You do not need to enter the account information when registering the extension, but can enter it in this field when <a href="/DevConsole/Guides/Test_Custom_Extension.md">testing the extension</a> later on.</li>
  <li>After filling out the basic custom extension information, click <strong>{{ book.DevConsole.cek_create }}</strong>.</li>
</ol>

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

<ol>
  <li>Click on the <strong>{{ book.DevConsole.cek_configuration }}</strong> tab above the extension information input UI.</li>
  <li>Enter the endpoint URI of the custom extension server in the <strong>{{ book.DevConsole.cek_service_endpoint_url }}</strong> item.
    <div class="note">
      <p><strong>Note!</strong></p>
      <p>An HTTP connection can be used for testing but an HTTPS connection is required for the official service. The custom extension server must use ports 80 and 443 for HTTP and HTTPS connections, respectively.</p>
    </div>
  </li>
  <li>If there is a need for linking the user account of the custom extension service and the Clova user account, select <strong>{{ book.DevConsole.cek_yes }}</strong> for the <strong>{{ book.DevConsole.cek_account_linking }}</strong> item. For more information on account linking, see <a href="#SetAccountLinking">setting up account linking</a>.</li>
  <li>Click the radio button of the <strong>{{ book.DevConsole.cek_ssl_certificate }}</strong> item. The custom extension server must use the certificate of an authorized certificate agency. (Self-signed certificates cannot be used.)</li>
  <li>Fill out the details for setting up the server connection and click <strong>{{ book.DevConsole.cek_save }}</strong>.</li>
</ol>

### Setting up account linking {#SetAccountLinking}

If a link between the user account of the custom extension service and the Clova user account is required, you must enter the [account linking](/Develop/Guides/Link_User_Account.md) information in [Setting up a server connection](#SetServerConnection).

Follow the steps below to enter the [information required](/Develop/Guides/Link_User_Account.md#RegisterAccountLinkingInfo) to set up account linking.

<ol>
  <img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_1.png" />
  <li>Select <strong>{{ book.DevConsole.cek_yes }}</strong> from the <strong>{{ book.DevConsole.cek_account_linking }}</strong> item.</li>
  <li>In the <strong>{{ book.DevConsole.cek_authorization_url }}</strong> item, enter the authorization URI where users can verify their account. The user will be directed to this URL when the custom extension is activated.</li>
  <li>If you want to allow a user to set up their own account right away, enter the URI for the account setup page in the <strong>{{ book.DevConsole.cek_configuration_url }}</strong> item.</li>
  <li>Enter the <strong>{{ book.DevConsole.cek_client_id }}</strong> required for an HTTP request when authenticating the user account. The client ID is the value created when <a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">building an authentication server</a>.</li>
  <li>In the <strong>{{ book.DevConsole.cek_privacy_policy_url }}</strong> item, enter the URI where the privacy policy on the custom extension service is provided. The details on this page will be shown later on the <strong>{{ book.DevConsole.ManageCustomExtensions }}</strong>.</li>
  <li>If the <strong>{{ book.DevConsole.cek_authorization_url }}</strong> or the page registered in the <strong>{{ book.DevConsole.cek_privacy_policy_url }}</strong> imports resources from another domain, add the corresponding domain address in the <strong>{{ book.DevConsole.cek_domain_list }}</strong> item.</li>
  <li>(Optional) If the usage scope of the access token, issued when linking the user account, is predefined, add the predefined scope in the <strong>{{ book.DevConsole.cek_scope }}</strong> item.</li>
  <img src="/DevConsole/Assets/Images/DevConsole-Custom_Extension_Accoun_Linking_Settings_2.png" />
  <li>In the <strong>{{ book.DevConsole.cek_access_token_uri }}</strong> item, enter the URI to get the service access token. Currently, <strong>only the code grant method is supported for grant type</strong>.</li>
  <li>In the <strong>{{ book.DevConsole.cek_refresh_token_uri }}</strong> item, enter the URI to renew the service access token in.</li>
  <li>After acquiring the service access token, enter the <strong>{{ book.DevConsole.cek_client_secret }}</strong> required for HTTP requests. The client secret is the value created when <a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">building an authentication server</a>.</li>
  <li>For <strong>{{ book.DevConsole.cek_client_authentication_scheme }}</strong>, set the value for implementing the authentication server interface.
    <ul>
      <li><strong>HTTP basic (Recommended)</strong>: Select when the credentials are sent in the HTTP header data to acquire the service access token.</li>
      <li><strong>Credentials in the request body</strong>: Select when the credentials are sent in the HTTP body data to acquire the service access token.</li>
    </ul>
  </li>
</ol>

<div id="RedirectURI" class="note">
  <p><strong>Note!</strong></p>
  <p>The client URI (redirect URI) to be redirected after account authentication is <code>{{ book.ServiceEnv.RedirectURIforAccountLinking }}</code> and it can be found in <strong>{{ book.DevConsole.cek_redirect_urls }}</strong>.</strong> The <a href="/Develop/Guides/Link_User_Account.md#BuildAuthServer">authorization server</a> must verify whether this value is identical to the <code>redirect_uri</code> value sent by the client.</p>
  <img src="/DevConsole/Assets/Images/DevConsole-Redirect_URI_for_Extension_Accoun_Linking.png" />
</div>
