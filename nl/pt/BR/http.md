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

# A interface HTTP
{: #usingHTTP}

Para sintetizar texto para voz com a interface de HTTP REST do {{site.data.keyword.texttospeechdatafull}} para o {{site.data.keyword.icp4dfull}}, você chama o método `GET` ou `POST /v1/synthesize`. Você especifica o texto que deve ser sintetizado e a voz e o formato para o áudio falado. Também é possível especificar um modelo de voz customizado que deve ser usado com a solicitação.
{: shortdesc}

Para obter mais informações sobre a interface HTTP, consulte a [Referência de API](https://{DomainName}/apidocs/text-to-speech-data){: external}.

## Sintetizando o texto para áudio
{: #synthesize}

Para sintetizar o texto para áudio, você chama uma das duas versões do método `/v1/synthesize` do serviço:

-   O método `GET /v1/synthesize` aceita o texto que deve ser sintetizado como um parâmetro de consulta `text` necessário. O tamanho máximo da solicitação é de 8 KB e inclui o texto de entrada, qualquer SSML especificado, a URL e os cabeçalhos.
-   O método `POST /v1/synthesize` aceita o texto que deve ser sintetizado como uma construção JSON no corpo necessário da solicitação. O tamanho máximo da solicitação é de 8 KB para a URL e os cabeçalhos e 5 KB para o texto de entrada enviado no corpo da solicitação. O limite de 5 KB inclui qualquer SSML especificado.

As duas versões do método `/v1/synthesize` têm os parâmetros a seguir em comum:

<table>
  <caption>Tabela 1. Parâmetros dos métodos <code>/v1/synthesize</code></caption>
  <tr>
    <th style="text-align:left; width:18%">Parâmetro</th>
    <th style="text-align:center; width:12%">Tipo</th>
    <th style="text-align:center; width:12%">Tipo de dados</th>
    <th style="text-align:left">Descrição</th>
  </tr>
  <tr>
    <td><code>aceitar</code><br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Consulta</td>
    <td style="text-align:center">Sequência</td>
    <td>
      Especifica o formato de áudio ou o tipo MIME solicitado, no qual o serviço deve retornar o áudio. Também é possível especificar esse valor com o cabeçalho de solicitação HTTP <code>Accept</code>. Codifique com URL o argumento para o parâmetro de consulta `accept`. Para obter mais informações, consulte [Formatos de áudio](/docs/services/text-to-speech-data?topic=text-to-speech-data-audioFormats).
    </td>
  </tr>
  <tr>
    <td><code>voice</code><br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Consulta</td>
    <td style="text-align:center">Sequência</td>
    <td>
      Especifica a voz na qual o texto deve ser falado no áudio. Use o método <code>/v1/vozes</code>para obter a
      lista atual de vozes suportadas. A voz padrão é
      <code>en-US_MichaelV3Voice</code>. Para obter mais informações, consulte [Idiomas e vozes](/docs/services/text-to-speech-data?topic=text-to-speech-data-voices).
    </td>
  </tr>
  <tr>
    <td><code>customization_id</code><br/><em>      Opcional
    </em></td>
    <td style="text-align:center">Consulta</td>
    <td style="text-align:center">Sequência</td>
    <td>
      Especifica um identificador exclusivo global (GUID) para um modelo de voz customizado que deve ser usado para a síntese. É garantido que um modelo de voz customizado especificado funcione somente se ele corresponder ao idioma da voz usada para a síntese. Se você incluir um ID de customização, a solicitação deverá ser realizada com credenciais para a instância do serviço que possui o modelo customizado. Omita o parâmetro para usar a voz especificada sem a customização. Para obter mais informações, consulte [Entendendo a customização](/docs/services/text-to-speech-data?topic=text-to-speech-data-customIntro).
    </td>
  </tr>
</table>

Também é possível usar o cabeçalho da solicitação `X-Watson-Metadata`, que associa um ID do cliente aos dados que são transmitidos com uma solicitação. Para obter mais informações, consulte [Segurança de informações](/docs/services/text-to-speech-data?topic=text-to-speech-data-information-security).

Se você especificar um parâmetro de consulta ou campo JSON inválido como parte da entrada para o método `/v1/synthesize`, o serviço retornará um cabeçalho de resposta `Warnings` que descreve e lista cada argumento inválido. A solicitação será bem-sucedida, apesar dos avisos.
{: note}

## Especificando o texto de entrada
{: #input}

Ambos os métodos `GET` e `POST /v1/synthesize` aceitam o texto de entrada sem formatação ou anotado com o SSML. As duas versões diferem principalmente na forma como você especifica o texto que deve ser sintetizado:

-   O método `GET /v1/synthesize` aceita o texto de entrada especificado pelo parâmetro de consulta `text`. Você especifica a entrada como um texto sem formatação ou como o SSML e ambos devem ser codificados com URL.
-   O método `POST /v1/synthesize` aceita o texto de entrada no corpo da solicitação. Você especifica a entrada com a construção JSON simples a seguir que encapsula o texto sem formatação ou o SSML. Também deve-se especificar um valor de `application/json` para o cabeçalho `Content-Type`.

    ```javascript
    {
      "text": ""
    }
    ```
    {: codeblock}

Embora os métodos `GET` e `POST` ofereçam funcionalidade equivalente, é sempre mais seguro transmitir o texto de entrada para o serviço com o método `POST`. Uma solicitação `POST` transmite a entrada no corpo da solicitação e uma solicitação `GET` expõe os dados na URL.

## Especificando a entrada do SSML
{: #ssml-http}

O Speech Synthesis Markup Language (SSML) é uma linguagem de marcações baseada em XML que foi projetada para fornecer anotações de texto para aplicativos de síntese de discurso, como o serviço {{site.data.keyword.texttospeechshort}}. É possível usar os elementos do SSML e seus atributos para obter maior controle sobre a síntese e a saída de áudio resultante.

Para obter mais informações sobre como usar o SSML para anotar o texto de entrada, consulte [Usando o SSML](/docs/services/text-to-speech-data?topic=text-to-speech-data-ssml). A documentação faz um inventário dos atributos e elementos do SSML suportados pelo serviço.

## Escapando Caracteres de Controle XML
{: #escape}

Como é possível enviar um texto de entrada que inclua as anotações do SSML baseadas em XML, o serviço valida todas as entradas para garantir que qualquer SSML esteja correto e bem formado. Portanto, deve-se escapar quaisquer caracteres de controle XML presentes no texto de entrada, independentemente de a entrada incluir o SSML. Use as sequências de escape ou as codificações de caracteres equivalentes da Tabela 2 em vez dos caracteres indicados.

<table style="width:80%">
  <caption>Tabela 2. Escapando caracteres de controle XML</caption>
  <tr>
    <th style="text-align:center; vertical-align:bottom; width:40%">Caractere</th>
    <th style="text-align:center; vertical-align:bottom; width:30%">Sequências de escape</th>
    <th style="text-align:center; vertical-align:bottom; width:30%">Codificação de caracteres</th>
  </tr>
  <tr>
    <td style="text-align:center"><code>&quot;</code><br/>(aspas duplas)</td>
    <td style="text-align:center"><code>&amp;quot;</code></td>
    <td style="text-align:center"><code>&amp;#34;</code></td>
  </tr>
  <tr>
    <td style="text-align:center"><code>'</code><br/>(apóstrofo ou aspas simples)</td>
    <td style="text-align:center"><code>&amp;apos;</code></td>
    <td style="text-align:center"><code>&amp;#39;</code></td>
  </tr>
  <tr>
    <td style="text-align:center"><code>&amp;</code><br/>(e comercial [símbolo &])</td>
    <td style="text-align:center"><code>&amp;amp;</code></td>
    <td style="text-align:center"><code>&amp;#38;</code></td>
  </tr>
  <tr>
    <td style="text-align:center"><code>&lt;</code><br/>(sinal de maior)</td>
    <td style="text-align:center"><code>&amp;lt;</code></td>
    <td style="text-align:center"><code>&amp;#60;</code></td>
  </tr>
  <tr>
    <td style="text-align:center"><code>&gt;</code><br/>(sinal de menor)</td>
    <td style="text-align:center"><code>&amp;gt;</code></td>
    <td style="text-align:center"><code>&amp;#62;</code></td>
  </tr>
</table>

Para obter mais informações sobre como o serviço valida o texto de entrada, consulte [Validação de SSML](/docs/services/text-to-speech-data?topic=text-to-speech-data-ssml#errors).

## Exemplos de texto de entrada
{: #httpExamples}

Os exemplos a seguir mostram como especificar texto de entrada com qualquer um dos métodos da interface HTTP. Eles também mostram como escapar caracteres de controle XML. Os exemplos incluem quebras de linha para a capacidade de leitura. *Não* inclua quebras de linha na entrada real.

### Exemplo de entrada com uma solicitação GET
{: #getExamples}

Os exemplos a seguir transmitem a entrada codificada com URL com o parâmetro de consulta `text` do método `GET /v1/synthesize`:

-   Entrada de texto sem formatação:

    ```
    text=This&20is&20the&20first&20sentence&20of&20the&20paragraph.&20Here
    &20is&20another&20sentence.&20Finally,&20this&20is&20the&20last&20sentence.
    ```
    {: codeblock}

-   Entrada SSML:

    ```
    text=%22%3Cp%3E%3Cs%3EThis%20is%20the%20first%20sentence%20of%20the%20%3C
    break%20time=%225s%22/%3E%20paragraph.%3C/s%3E%3Cs%3EHere%20is%20another
    %20sentence.%3C/s%3E%3Cs%3EFinally,%20this%20is%20the%20last%20sentence.
    %3C/s%3E%3C/p%3E%22
    ```
    {: codeblock}

### Exemplo de entrada com uma solicitação de POST
{: #postExamples}

Os exemplos a seguir transmitem a entrada no corpo do método `POST /v1/synthesize`:

-   Entrada de texto sem formatação:

    ```javascript
    {
      "text": "Essa é a primeira sentença do parágrafo. Aqui está outra.
        sentença. Finalmente, essa é a última sentença."
    }
    ```
    {: codeblock}

-   Entrada SSML:

    ```javascript
    {
      "text": "<p><s>Essa é a primeira sentença do <break time=\"5s\"/>
        parágrafo.</s><s>Aqui está outra sentença.</s><s>Finalmente, essa é a última sentença.</s></p>"
    }
    ```
    {: codeblock}

### Exemplo de entrada com caracteres de controle XML
{: #xmlExamples}

O exemplo a seguir envia uma sentença para o método `POST /v1/synthesize`. O exemplo escapa adequadamente os caracteres XML integrados.

```
"O que aprendi" ele perguntou. "Tudo!"
```
{: codeblock}

```javascript
{
  "text": "&quot;O que aprendi?&quot; ele perguntou. &quot;Tudo!&quot;"
    }
```
{: codeblock}
