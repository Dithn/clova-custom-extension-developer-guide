<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

<!-- Start of the shared content: CEKHTTPMessageReference -->

# HTTPメッセージ
CEKとExtensionは、HTTP/1.1プロトコルを使用して、基本的なHTTPリクエストとHTTPレスポンスのやり取りを行います。CEKとExtensionが通信する際、HTTPメッセージのボディには、JSON形式のメッセージが含まれます。ここでは、CEKとExtensionがやり取りするHTTPメッセージの構成について説明します。

* [HTTPヘッダー](#HTTPHeader)
* [HTTPボディ](#HTTPBody)

## HTTPヘッダー {#HTTPHeader}
CEKからExtensionに解析されたユーザーの発話情報を送信するとき、HTTPリクエストを使用します。そのとき、HTTPリクエストのヘッダーは、以下のように構成されます。

{% raw %}

```
POST /APIpath HTTP/1.1
Host: your.extension.endpoint
Content-Type: application/json;charset-UTF-8
Accept: application/json
Accept-Charset: utf-8
SignatureCEK：{{ SignatureCEK }}
```
{% endraw %}

* HTTP/1.1バージョンでHTTPS通信し、POSTメソッドを使用します。
* Hostとリクエストパスは、Extensionの開発者があらかじめ定義したURIに設定されます。
* ボディのデータフォーマットはJSONで、UTF-8エンコーディングを使用します。
* `SignatureCEK`フィールドとRSA公開鍵を使用して、Clovaから送信されたリクエストかどうかを検証することができます。

逆に、ExtensionからClovaに処理結果を返すときには、HTTPレスポンスを使用します。そのとき、HTTPレスポンスのヘッダーは、以下のように構成します。
{% raw %}
```
HTTP/1.1 200 OK
Content-Type: application/json;charset-UTF-8
```
{% endraw %}
* CEKからのHTTPリクエストに対するレスポンスとして、処理結果を返します。
* ボディのデータはJSON形式で、UTF-8エンコーディングを使用します。

## HTTPボディ {#HTTPBody}
HTTPリクエストメッセージとレスポンスメッセージのボディはJSON形式で、解析されたユーザーの発話情報や、Extensionの処理結果が含まれています。それぞれのメッセージの構成は、使用するExtensionの種類によって異なります。メッセージ構成の詳細については、[Custom Extensionのメッセージ](#CustomExtMessage)を参照してください。

<!-- End of the shared content -->
