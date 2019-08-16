---

copyright:
  years: 2019
lastupdated: "2019-06-24"

subcollection: text-to-speech-data

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# WebSocket 介面
{: #usingWebSocket}

若要使用 {{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} 的 WebSocket 介面將文字合成為語音，首先請藉由呼叫其 `/v1/synthesize` 方法來建立與此服務的連線。然後，透過連線將要合成的文字傳送至服務，作為 JSON 文字訊息。服務在完成處理要求時，會自動關閉 WebSocket 連線。
{: shortdesc}

合成要求和回應週期包括下列步驟：

1.  [開啟連線](#WSopen)。
1.  [傳送輸入文字](#WSsend)。
1.  [接收回應](#WSreceive)。

WebSocket 介面接受相同的輸入，並產生與 HTTP 介面的 `GET` 和 `POST /v1/synthesize` 方法相同的結果。不過，WebSocket 介面支援使用 SSML `<mark>` 元素來識別使用者指定的標記在音訊中的位置。它也可以針對輸入文字的所有字串傳回計時資訊。如需相關資訊，請參閱[取得字組計時](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing)。

隨後的程式碼範例 Snippet 是以 JavaScript 撰寫，並以 HTML5 WebSocket API 為基礎。如需 WebSocket 通訊協定的相關資訊，請參閱網際網路工程工作小組 (IETF) 的 [Request for Comments (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}。
{: note}

## 開啟連線
{: #WSopen}

{{site.data.keyword.texttospeechshort}} 使用 WebSocket 安全 (WSS) 通訊協定使 `/v1/synthesize` 方法在下列端點可用。如需 URL 元件的相關資訊，請參閱[向服務提出要求](/docs/services/text-to-speech-data?topic=text-to-speech-data-making-requests)。

```
wss://{icp4d_cluster_host}{:port}/text-to-speech/{release}/instances/{instance_id}/api/v1/synthesize
```
{: codeblock}

文件中的範例參照此端點的形式是 `{ws_url}`。
{: note}

WebSocket 用戶端會呼叫此方法並搭配下列查詢參數，來建立與服務的鑑別連線。

<table>
  <caption>表 1. <code>/v1/synthesize</code> 方法的參數</caption>
  <tr>
    <th style="text-align:left; width:23%">參數</th>
    <th style="text-align:center; width:12%">資料類型</th>
    <th style="text-align:left">說明</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      傳遞有效的存取記號以向服務鑑別。您必須在存取記號到期之前使用它。
      </td>
  </tr>
  <tr>
    <td><code>voice</code><br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td>
      指定要在音訊中說出的文字語音。省略此參數，會使用預設語音 `en-US_MichaelV3Voice`。
          如需相關資訊，請參閱[語言和語音](/docs/services/text-to-speech-data?topic=text-to-speech-data-voices)。
</td>
  </tr>
  <tr>
    <td><code>customization_id</code><br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td>
      為要用於合成的自訂語音模型指定廣域唯一 ID (GUID)。只有在自訂語音模型符合用於合成之語音的語言時，才保證此模型可以運作。如果您包含了自訂作業 ID，則必須使用擁有自訂模型之服務實例的認證來提出要求。省略此參數，會使用沒有自訂作業的指定語音。如需相關資訊，請參閱[瞭解自訂作業](/docs/services/text-to-speech-data?topic=text-to-speech-data-customIntro)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>選用</em></td>
    <td style="text-align:center">字串</td>
    <td style="text-align:left">
      建立客戶 ID 與透過連線傳遞之資料的關聯。此參數接受引數 <code>customer_id={id}</code>，其中 <code>id</code> 是要與資料相關聯的隨機或通用字串。您必須將引數以 URL 編碼為查詢參數，例如，`customer_id%3dmy_ID`。依預設，沒有客戶 ID 與資料相關聯。如需相關資訊，請參閱[資訊安全](/docs/services/text-to-speech-data?topic=text-to-speech-data-information-security)。</td>
  </tr>
</table>

JavaScript 程式碼的下列 Snippet 會開啟與服務的連線。呼叫 `/v1/synthesize` 方法會傳遞 `access_token` 和 `voice` 查詢參數，而後者會引導服務使用美式英文 Allison 語音。建立連線之後，就會定義事件接聽器（`onOpen`、`onClose` 等等），以回應服務中的事件。

```javascript
var my_access_token = '{token}';
var wsURI = '{ws_url}/v1/synthesize'
  + '?access_token=' + my_access_token
  + '&voice=en-US_AllisonV3Voice';
var websocket = new WebSocket(wsURI);

websocket.onopen = function(evt) { onOpen(evt) };
websocket.onclose = function(evt) { onClose(evt) };
websocket.onmessage = function(evt) { onMessage(evt) };
websocket.onerror = function(evt) { onError(evt) };
```
{: codeblock}

## 傳送輸入文字
{: #WSsend}

若要合成文字，用戶端會使用下列參數，將簡單的 JSON 文字訊息傳遞至服務。

<table>
  <caption>表 2. JSON 文字訊息的參數</caption>
  <tr>
    <th style="text-align:left; width:23%">參數</th>
    <th style="text-align:center; width:12%">資料類型</th>
    <th style="text-align:left">說明</th>
  </tr>
  <tr>
    <td><code>text</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td>
      提供要合成的文字。用戶端可以傳遞純文字，或使用「語音合成標記語言 (SSML)」標註的文字。用戶端可以搭配要求最多傳送 5 KB 的輸入文字。此限制包括您指定的任何 SSML。如需相關資訊，請參閱[指定輸入文字](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingHTTP#input)，以及在它之後的小節。<br/><br/>
      SSML 輸入也可以包含 <code>&lt;mark&gt;</code> 元素。
      如需相關資訊，請參閱[指定 SSML 標記](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing#mark)。
    </td>
  </tr>
  <tr>
    <td><code>accept</code><br/><em>必要</em></td>
    <td style="text-align:center">字串</td>
    <td>
      指定音訊的要求格式（MIME 類型）。請使用 `*/*` 來要求預設音訊格式，即 <code>audio/ogg;codecs=opus</code>。如需相關資訊，請參閱[音訊格式](/docs/services/text-to-speech-data?topic=text-to-speech-data-audioFormats)。</td>
  </tr>
  <tr>
    <td><code>timings</code><br/><em>選用</em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      指定服務要針對輸入文字的所有字串傳回字組計時資訊。服務會傳回一個輸入記號的開始及結束時間。請指定 <code>words</code> 作為陣列的 lone 元素，來要求字組計時。指定空陣列或省略參數，則不接收字組計時。
      如需相關資訊，請參閱[取得字組計時](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing#timing)。<em>日文輸入文字不支援此參數。</em>
    </td>
  </tr>
</table>

JavaScript 程式碼的下列 Snippet 會傳遞簡單的“Hello world”訊息作為輸入文字，並要求音訊的預設格式。呼叫包含在針對用戶端定義的 `onOpen` 函數中，以確保只在建立連線之後才會傳送它們。

```javascript
function onOpen(evt) {
  var message = {
    text: 'Hello world',
    accept: '*/*'
  };
  websocket.send(JSON.stringify(message));
}
```
{: codeblock}

服務會傳送一則文字訊息確認音訊回應的格式，來回應此訊息。下列回應確認預設音訊格式。

```javascript
{
  'binary_streams': [
    {
      content_type: 'audio/ogg;codecs=opus'
    }
  ]
}
```
{: codeblock}

## 接收回應
{: #WSreceive}

在確認音訊格式之後，服務會以指示的格式將合成音訊當作二進位資料串流傳送。如果出現下列情況，服務會傳送計時資訊，作為一則以上的文字訊息：

-   輸入文字包括一個以上的 SSML `<mark>` 元素。
-   您指定 `timings` 參數與要求搭配。

服務也可以傳送具有警告或錯誤的文字訊息。服務在完成合成輸入文字時，會自動關閉 WebSocket 連線。

用戶端需要將來自服務的二進位回應附加到透過連線接收的音訊結果。它可以處理文字訊息，方法是回應它們、顯示它們，或擷取它們以供應用程式使用（例如，如果它們包含標記位置）。下列簡單的 `onMessage` 函數範例會根據類型，將從服務接收的文字和二進位訊息附加到適當的變數。`onClose()` 函數執行時，將接收到整個音訊串流。

```javascript
var messages;
var audioStream;

function onMessage(evt) {
  if (typeof evt.data === string) {
    messages += evt.data;
  } else {
    console.log('Received ' + evt.data.size() + ' binary bytes');
    audioStream += evt.data;
  }
}

function onClose(evt) {
  // The audio stream is complete.
}
```
{: codeblock}

## WebSocket 回覆碼
{: #returnCodes}

服務可以透過 WebSocket 連線，將下列回覆碼傳送至用戶端：

-   `1000` 指出正常關閉連線，這表示已完成建立連線的目的。
-   `1002` 指出由於通訊協定錯誤，服務正在關閉連線。
-   `1006` 指出連線異常關閉。
-   `1009` 指出訊框大小超出 4 MB 限制。
-   `1011` 指出服務正在終止連線，因為它遇到的非預期狀況阻止它完成要求，例如無效的引數。回覆碼也可以指出輸入文字太大。

如果 Socket 由於錯誤而關閉，則服務會在關閉之前，將表單 `{"error": "Specific error message"}` 的參考訊息傳送給用戶端。服務也會針對不明參數傳送不嚴重的警告訊息。

## 範例錯誤及警告訊息
{: #returnErrors}

下列範例顯示錯誤回應。它們包括 JSON 文字訊息，以及來自用戶端的 `onClose` 回呼方法的格式化訊息。因為連線已關閉，所以格式化訊息的開頭是布林 `true`。它們也包括導致關閉的 WebSocket 錯誤碼。

-   此範例顯示錯誤訊息，指出 `accept` 參數的引數無效：

    ```javascript
    {
      "error": "Unsupported mimetype. Supported mimetypes are: ['application/json', 'audio/flac', ...]"
    }
    (True, 1011, u'see the previous message for the error details.')
    ```
    {: codeblock}

-   此範例顯示錯誤訊息，指出遺漏 `text` 參數：

    ```javascript
    {
      "error": "Required parameter \"text\" is missing."
    }
    (True, 1011, u'see the previous message for the error details.')
    ```
    {: codeblock}

下列範例針對不明參數顯示警告回應，在此情況下，是名稱為 `invalid-parameter` 的不明參數。它未包含第二則訊息，因為連線不是由警告關閉。

```javascript
{
  "warnings": "Unknown arguments: invalid-parameter."
}
```
{: codeblock}

如需 WebSocket 回覆碼的相關資訊，請參閱網際網路工程工作小組 (IETF) 的 [Request for Comments (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}。
