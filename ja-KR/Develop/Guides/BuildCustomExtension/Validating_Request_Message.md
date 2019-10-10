<!-- Note! This content includes shared parts. Therefore, when you update this, you should beware of synchronization. -->

### リクエストメッセージを検証する {#RequestMessageValidation}

<!-- Start of the shared content: CEKRequestMessageValidation -->

ExtensionがCEKからのHTTPリクエストを受信するとき、そのリクエストが第三者ではなく、Clovaから送信された信頼できるリクエストかどうかを検証する必要があります。[HTTPヘッダー](/Develop/References/HTTP_Message.md#HTTPHeader)にある`SignatureCEK`とRSA公開鍵を使用して、以下のようにリクエストメッセージを検証してください。

1. Clovaの署名用RSA公開鍵を以下のURIからダウンロードします。<br />
  ```
  {{ book.ServiceEnv.PublicKeyURIforCEKMessageValidation }}
  ```
2. [HTTPヘッダー](/Develop/References/HTTP_Message.md#HTTPHeader)から`SignatureCEK`フィールドの値を取得します。<br />
  `SignatureCEK`フィールドの値は、HTTPリクエストメッセージのボディをBase64エンコードした<a href="https://tools.ietf.org/html/rfc3447" target="_blank">RSA PKCS#1 v1.5</a>署名（signature）です。
3. ステップ1でダウンロードしたRSA公開鍵を用いて、ステップ2で取得した`SignatureCEK`ヘッダーを以下のように<a href="https://tools.ietf.org/html/rfc3447#section-5.2" target="_blank">検証（verify）</a>してください。

以下は、リクエストメッセージを検証するコードサンプルです。
```java
String signatureStr = req.getHeader("SignatureCEK");
byte[] body = getBody(req);

String publicKeyStr = downloadPublicKey();
publicKeyStr = publicKeyStr.replaceAll("\\n", "")
    .replaceAll("-----BEGIN PUBLIC KEY-----", "")
    .replaceAll("-----END PUBLIC KEY-----", "");
X509EncodedKeySpec pubKeySpec = new X509EncodedKeySpec(Base64.getDecoder().decode(publicKeyStr));

KeyFactory keyFactory = KeyFactory.getInstance("RSA");
PublicKey pubKey = keyFactory.generatePublic(pubKeySpec);
Signature sig = Signature.getInstance("SHA256withRSA");
sig.initVerify(pubKey);
sig.update(body);

byte[] signature = Base64.getDecoder().decode(signatureStr);
boolean valid = sig.verify(signature);
```

<div class="note">
  <p><strong>メモ</strong></p>
  <p>メッセージの検証に失敗した場合、そのリクエストを破棄する必要があります。</p>
</div>

<!-- End of the shared content -->
