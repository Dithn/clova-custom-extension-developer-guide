<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: CEKHTTPMessageReference -->

# HTTP messages
CEK and the extensions communicate via the HTTP/1.1 protocol and exchange basic HTTP requests and HTTP responses. During this communication, a JSON format message is contained in the HTTP message body. This section explains the configuration of HTTP messages exchanged between CEK and the extension.

* [HTTP header](#HTTPHeader)
* [HTTP body](#HTTPBody)

## HTTP header {#HTTPHeader}
An HTTP request is used when CEK sends the analyzed information of a user utterance to the extension. The HTTP request header is configured as follows:

{% raw %}

```
POST /APIpath HTTP/1.1
Host: your.extension.endpoint
Content-Type: application/json;charset-UTF-8
Accept: application/json
Accept-Charset: utf-8
SignatureCEK: {{ SignatureCEK }}
```
{% endraw %}

* HTTPS communication is performed with HTTP/1.1 and uses the POST method.
* The values for the host and request target path are filled with the URI defined by you.
* The data type of the body is in JSON format and is UTF-8 encoded.
* You can validate whether or not a request is sent from Clova using the `SignatureCEK` field and the RSA public key.

However, an HTTP response is used when the extension sends the processed results to Clova. You can configure the HTTP response header as follows:
{% raw %}
```
HTTP/1.1 200 OK
Content-Type: application/json;charset-UTF-8
```
{% endraw %}
* The processed results are returned as a response to the HTTP request from CEK.
* The data type of the body is in JSON format and uses UTF-8 encoding.

## HTTP body {#HTTPBody}
Both the body of the HTTP request and response messages are in JSON format, and it contains the information about the user utterance analysis or the processed results of the extension. The configuration of each message varies depending on the extension type. For more information on message configurations, see [Custom extension messages](#CustomExtMessage).

<!-- End of the shared content -->
