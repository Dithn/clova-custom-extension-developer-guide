* **Extension server**

	Extension server refers to a web server where the extension is executed. This server is registered to the Clova developer console. The extension server must return an appropriate response by handling the analyzed results of user utterances or default intents sent by Clova. When handling requests, the extension server must use the **HTTPS protocol**.

	<div class="note">
		<p><strong>Note!</strong></p>
		<p>An HTTP connection can be used for testing but an HTTPS connection is required for the official service. The extension server must use ports 80 and 443 for HTTP and HTTPS connections respectively.</p>
	</div>
