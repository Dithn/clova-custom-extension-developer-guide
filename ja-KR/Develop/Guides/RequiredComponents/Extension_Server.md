* **Extensionサーバー**

	Extensionが実行される Webサーバーです。Clova Developer Centerに登録されます。Clovaからユーザーの音声入力を解析した結果や、デフォルトで提供されるインテントを受け取ると、Extensionサーバーはそれを処理して適切な応答を返す必要があります。Extensionサーバーは、リクエストを処理するとき、**HTTPSプロトコル**を使用する必要があります。

	<div class="note">
		<p><strong>メモ</strong></p>
		<p>テスト段階ではHTTPも使用できますが、正式なサービスのためにはHTTPSを使用する必要があります。Extensionサーバーは、HTTPで80ポート、HTTPSで443ポートに設定してください。</p>
	</div>
