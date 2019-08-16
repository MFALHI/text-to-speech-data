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

# WebSocket 接口
{: #usingWebSocket}

要使用 {{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} 的 WebSocket 接口将文本合成为语音，可首先通过调用其 `/v1/synthesize` 方法来建立与服务的连接。然后，通过该连接将要合成的文本作为 JSON 文本消息发送到服务。服务完成请求处理后，会自动关闭该 WebSocket 连接。
{: shortdesc}

合成请求和响应周期包含以下步骤：

1.  [打开连接](#WSopen)。
1.  [发送输入文本](#WSsend)。
1.  [接收响应](#WSreceive)。

WebSocket 接口接受的输入和生成的结果与 HTTP 接口的 `GET` 和 `POST /v1/synthesize` 方法完全相同。但是，WebSocket 接口支持使用 SSML `<mark>` 元素来标识用户指定的标记在音频中的位置。此接口还可以返回输入文本中所有字符串的计时信息。有关更多信息，请参阅[获取词计时](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing)。

以下示例代码片段是用 JavaScript 编写的，并基于 HTML5 WebSocket API。有关 WebSocket 协议的更多信息，请参阅因特网工程任务组织 (IETF) 的 [Request for Comments (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}。
{: note}

## 打开连接
{: #WSopen}

{{site.data.keyword.texttospeechshort}} 使用 WebSocket 安全 (WSS) 协议使 `/v1/synthesize` 方法在以下端点可用。有关 URL 组件的更多信息，请参阅[向服务发出请求](/docs/services/text-to-speech-data?topic=text-to-speech-data-making-requests)。

```
wss://{icp4d_cluster_host}{:port}/text-to-speech/{release}/instances/{instance_id}/api/v1/synthesize
```
{: codeblock}

文档中的示例引用此端点的形式是 `{ws_url}`。
{: note}

WebSocket 客户机调用此方法并使用以下查询参数来建立与服务的已认证连接。

<table>
  <caption>表 1. <code>/v1/synthesize</code> 方法的参数</caption>
  <tr>
    <th style="text-align:left; width:23%">参数</th>
    <th style="text-align:center; width:12%">数据类型</th>
    <th style="text-align:left">描述</th>
  </tr>
  <tr>
    <td style="text-align:left"><code>access_token</code>
      <br/><em>必需</em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      传递有效的访问令牌以向服务认证。必须使用未到期的访问令牌。</td>
  </tr>
  <tr>
    <td><code>voice</code><br/><em>      可选
    </em></td>
    <td style="text-align:center">String</td>
    <td>
      指定音频中读文本的声音。省略此参数将使用缺省声音 `en-US_MichaelV3Voice`。有关更多信息，请参阅[语言和声音](/docs/services/text-to-speech-data?topic=text-to-speech-data-voices)。</td>
  </tr>
  <tr>
    <td><code>customization_id</code><br/><em>      可选
    </em></td>
    <td style="text-align:center">String</td>
    <td>
      指定用于合成的定制声音模型的全局唯一标识 (GUID)。仅当定制声音模型与用于合成的声音的语言相匹配时，才可保证该模型正常工作。如果包含定制标识，那么必须使用拥有定制模型的服务实例的凭证来发出请求。省略此参数可使用没有任何定制的指定声音。有关更多信息，请参阅[了解定制](/docs/services/text-to-speech-data?topic=text-to-speech-data-customIntro)。
    </td>
  </tr>
  <tr>
    <td style="text-align:left"><code>x-watson-metadata</code>
      <br/><em>      可选
    </em></td>
    <td style="text-align:center">String</td>
    <td style="text-align:left">
      将客户标识与通过连接传递的数据相关联。此参数接受自变量 <code>customer_id={id}</code>，其中 <code>id</code> 是要与数据关联的随机或通用字符串。必须对该自变量进行 URL 编码，以转入该参数，例如 `customer_id%3dmy_ID`。缺省情况下，没有客户标识与数据相关联。有关更多信息，请参阅[信息安全](/docs/services/text-to-speech-data?topic=text-to-speech-data-information-security)。</td>
  </tr>
</table>

以下 JavaScript 代码片段将打开与服务的连接。对 `/v1/synthesize` 方法的调用传递了 `access_token` 和 `voice` 查询参数，后者指示服务使用美国英语 Allison 声音。建立连接后，定义了事件侦听器（`onOpen`、`onClose` 等），以响应来自服务的事件。

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

## 发送输入文本
{: #WSsend}

要合成文本，客户机会使用以下参数将简单的 JSON 文本消息传递给服务。

<table>
  <caption>表 2. JSON 文本消息的参数</caption>
  <tr>
    <th style="text-align:left; width:23%">参数</th>
    <th style="text-align:center; width:12%">数据类型</th>
    <th style="text-align:left">描述</th>
  </tr>
  <tr>
    <td><code>text</code><br/><em>必需</em></td>
    <td style="text-align:center">String</td>
    <td>
      提供要合成的文本。客户机可以传递纯文本或使用语音合成标记语言 (SSML) 进行注释的文本。客户机在请求中最多可传递 5 KB 的输入文本。此限制包括指定的任何 SSML。有关更多信息，请参阅[指定输入文本](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingHTTP#input)及其后面的各部分内容。<br/><br/>
      SSML 输入还可以包含 <code>&lt;mark&gt;</code> 元素。有关更多信息，请参阅[指定 SSML 标记](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing#mark)。
    </td>
  </tr>
  <tr>
    <td><code>accept</code><br/><em>必需</em></td>
    <td style="text-align:center">String</td>
    <td>
      指定请求的音频格式（MIME 类型）。使用 `*/*` 可请求缺省音频格式 <code>audio/ogg;codecs=opus</code>。有关更多信息，请参阅[音频格式](/docs/services/text-to-speech-data?topic=text-to-speech-data-audioFormats)。
    </td>
  </tr>
  <tr>
    <td><code>timings</code><br/><em>      可选
    </em></td>
    <td style="text-align:center">String[ ]</td>
    <td>
      指定服务将返回输入文本中所有字符串的词计时信息。服务会返回输入中每个标记的开始时间和结束时间。将 <code>words</code> 指定为数组的唯一元素，以请求词计时。指定空数组或省略此参数时，不会收到任何词计时信息。有关更多信息，请参阅[获取词计时](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing#timing)。<em>日语输入文本不支持此参数。</em>
    </td>
  </tr>
</table>

以下 JavaScript 代码片段将简单的“Hello world”消息作为输入文本传递，并请求音频的缺省格式。调用包含在为客户机定义的 `onOpen` 函数中，以确保仅在建立连接后才发送这些调用。

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

服务通过发送用于确认音频响应格式的文本消息来响应此消息。以下响应将确认缺省音频格式。

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

## 接收响应
{: #WSreceive}

服务确认音频格式后，会将合成音频作为所示格式的二进制数据流发送。如果存在以下情况，服务会将计时信息作为一条或多条文本消息发送：

-   输入文本包含一个或多个 SSML `<mark>` 元素。
-   在请求中指定了 `timings` 参数。

服务还可以发送包含警告或错误的文本消息。服务完成输入文本的合成后，会自动关闭 WebSocket 连接。

客户机需要将来自服务的二进制响应附加到通过连接收到的音频结果。客户机可以通过响应或显示文本消息，或者通过捕获文本消息供应用程序使用（例如，如果消息包含标记位置），对这些消息进行处理。以下 `onMessage` 函数的简单示例将从服务收到的文本和二进制消息，根据其类型附加到相应的变量。`onClose()` 函数执行时，已收到整个音频流。

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

## WebSocket 返回码
{: #returnCodes}

服务可以通过 WebSocket 连接向客户机发送以下返回码：

-   `1000` 指示连接正常关闭，这意味着已实现建立连接的目的。
-   `1002` 指示由于协议错误，服务正在关闭连接。
-   `1006` 指示连接异常关闭。
-   `1009` 指示帧大小超过 4 MB 限制。
-   `1011` 指示服务正在终止连接，因为服务遇到了阻止其执行请求的意外状况，例如无效自变量。此返回码还可能指示输入文本太大。

如果套接字关闭并返回错误，那么在关闭之前，服务会向客户机发送格式为 `{"error": "Specific error message"}` 的参考消息。此外，对于未知参数，服务会发送非致命警告消息。

## 示例错误和警告消息
{: #returnErrors}

以下示例显示了错误响应。其中包括来自客户机的 `onClose` 回调方法的 JSON 文本消息和格式化消息。格式化消息以布尔值 `true` 开头，因为连接已关闭。此外，还包含导致关闭的 WebSocket 错误代码。

-   以下示例显示了有关 `accept` 参数自变量无效的错误消息：

    ```javascript
    {
      "error": "Unsupported mimetype. Supported mimetypes are: ['application/json', 'audio/flac', ...]"
    }
    (True, 1011, u'see the previous message for the error details.')
    ```
    {: codeblock}

-   以下示例显示了有关缺少 `text` 参数的错误消息：

    ```javascript
    {
      "error": "Required parameter \"text\" is missing."
    }
    (True, 1011, u'see the previous message for the error details.')
    ```
    {: codeblock}

以下示例显示了警告响应，在本例中是有关名为 `invalid-parameter` 的参数未知的警告响应。响应中并未包含第二条消息，因为该警告未导致连接关闭。

```javascript
{
  "warnings": "Unknown arguments: invalid-parameter."
}
```
{: codeblock}

有关 WebSocket 返回码的更多信息，请参阅因特网工程任务组织 (IETF) 的 [Request for Comments (RFC) 6455](http://tools.ietf.org/html/rfc6455){: external}。
