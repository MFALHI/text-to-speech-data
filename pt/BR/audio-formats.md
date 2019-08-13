---

copyright:
  years: 2019
lastupdated: "2019-06-11"

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

# Formatos de áudio
{: #audioFormats}

O {{site.data.keyword.texttospeechdatafull}} para {{site.data.keyword.icp4dfull}} pode retornar o áudio sintetizado em uma série de formatos. Para a maioria dos formatos, o serviço retorna o áudio com uma taxa de amostragem padrão de 22.050 Hz. Para alguns formatos, é possível ou necessário especificar a taxa de amostragem do áudio. O serviço sempre retorna o áudio de canal único para todos os formatos.
{: shortdesc}

## Formatos de áudio suportados
{: #formatsSupported}

A Tabela 1 lista os formatos de áudio (tipos MIME) nos quais é possível solicitar o áudio sintetizado. Por padrão, o serviço retorna o áudio no formato Ogg, com o codec Opus (`audio/ogg;codecs=opus`).

<table>
  <caption>Tabela 1. Formatos de áudio suportados</caption>
  <tr>
    <th style="text-align:left; width:25%">Formato de áudio</th>
    <th style="text-align:left">Descrição</th>
  </tr>
  <tr>
    <td>
      <code>áudio / básico</code>
    </td>
    <td>
      <em>Áudio Básico</em>, um formato de áudio de canal único e com lossico que é
      codificado pelo uso de dados de 8 bits u-law (ou mulei) que são amostrados em 8 kHz.
      Esse formato fornece um tipo de mídia de denominador comum mais baixo. Para obter mais informações, consulte o IETF [Solicitação de comentários (RFC) 2046](https://tools.ietf.org/html/rfc2046) e [iana.org/assignments/media-types/audio/basic](http://www.iana.org/assignments/media-types/audio/basic).
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / flac</code>
    </td>
    <td>
      <em>Free Lossless Audio Codec (FLAC)</em> (<code>.flac</code>),
      um formato de codificação de áudio compactado e sem perdas. Para obter mais informações, consulte [FLAC](https://wikipedia.org/wiki/FLAC).
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / l16; taxa = { rate }</code>
    </td>
    <td>
      <em>Módulos de Código de Pulso de 16 bits (PCM) lineares</em>, um descompactado
      formato de dados de áudio (geralmente <code>.raw</code>ou <code>.pcm</code>). Para obter mais informações, consulte o Internet Engineering Task Force (IETF) [Solicitação de comentários (RFC) 2586](https://tools.ietf.org/html/rfc2586) e [Modulação de código de pulso](https://wikipedia.org/wiki/Pulse-code_modulation).<br/><br/>
      Deve-se
      especificar a taxa de amostragem com esse formato de áudio. Por exemplo,
      especifique <code>audio/l16;rate=16000</code> para o áudio amostrado em
      16 kHz. Também é possível, opcionalmente, especificar a ordenação como
      <code>audio/l16;rate={rate};endianness=big-endian</code> ou
      <code>audio/l16;rate={rate};endianness=little-endian</code>.
      O padrão é little endian.
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/mp3</code><br/>
      <code>audio/mpeg</code>
    </td>
    <td>
      <em>MP3</em>ou <em>Motion Picture Experts Group (MPEG)</em>, uma lossy
      o formato de compactação de dados (MP3 e MPEG se referem ao mesmo formato).
      Para obter mais informações, consulte [MP3](https://wikipedia.org/wiki/MP3).
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / mulaw; taxa = { rate }</code>
    </td>
    <td>
      <em>8-bit mu-law (or u-law) audio</em>, um formato de áudio de canal único com perdas
      que é codificado usando dados u-law (ou mu-law) de 8 bits. Deve-se
      especificar a taxa de amostragem com esse formato de áudio. Para obter mais informações, consulte
[Algoritmo M-law](https://wikipedia.org/wiki/M-law_algorithm).
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/ogg</code><br/>
      <code>audio/ogg;codecs=opus</code><br/>
      <code>audio/ogg;codecs=vorbis</code>
    </td>
    <td>
      <em>Ogg format</em> (<code>.ogg</code>), um formato grátis e de contêiner aberto
      mantido pela Xiph.org Foundation. Para obter mais informações, consulte [xiph.org/ogg/](https://www.xiph.org/ogg/).
      É possível solicitar fluxos de áudio compactados com os codecs
      a seguir:
      <ul style="margin-left:20px; padding:0px;">
        <li style="margin:10px 0px; line-height:120%;">
          <em>Opus</em>. Para obter mais informações, consulte [opus-codec.org](https://www.opus-codec.org/) e [Opus (formato de áudio)](https://wikipedia.org/wiki/Opus_%28audio_format%29).
          Procure especialmente na seção <em>Contêineres</em>.
        </li>
        <li style="margin:10px 0px; line-height:120%;">
          <em>Vorbis</em>. Para obter mais informações, consulte [xiph.org/vorbis](https://xiph.org/vorbis/) e [Vorbis](https://wikipedia.org/wiki/Vorbis).
        </li>
      </ul>
      Ambos os codecs são livres, abertos, formatos de compactação de áudio com lossy. Opus
      é o codec preferencial, mas conforme a especificação Ogg, o serviço
      retorna o áudio no formato Vorbis se você omitir o codec. Se você
      omitir um formato de áudio completamente, o serviço retorna o áudio em
      O formato Ogg com o codec de Opus por padrão.
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / wav</code>
    </td>
    <td>
      <em>Waveform Audio File Format (WAV)</em>(<code>.wav</code>) é
      um formato de contêiner padrão que é freqüentemente usado para descompactado
      Os bitstreams de áudio, mas também podem conter áudio compactado. Devido a
      a natureza de fluxo do áudio retornado, o arquivo WAV que é
      gerado pode não funcionar em todos os leitores de áudio. Especificamente,
      o atributo <code>numSamples</code>no cabeçalho do arquivo
      é configurado como <code>0</code>independentemente do comprimento do áudio.
      Para obter mais informações, consulte [WAV](https://wikipedia.org/wiki/WAV).
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/webm</code><br/>
      <code>audio/webm;codecs=opus</code><br/>
      <code>áudio / webm; codecs=vorbis</code>
    </td>
    <td>
      <em>Web Media (WebM)</em>(<code>.webm</code>), um arquivo de mídia aberto
      formato que fornece suporte para fluxos de áudio que são compactados
      com os codecs de áudio Opus e Vorbis. Se você omitir o codec, o
      O serviço retorna o áudio no formato Opus. Para obter mais informações, consulte [webmproject.org](https://www.webmproject.org/).
    </td>
  </tr>
</table>

## Especificando um Formato de Áudio
{: #formatSpecify}

Especificar um formato de áudio é opcional. Por padrão, o serviço retorna áudio no formato `áudio / ogg; codecs=opus`. Mas é possível especificar um formato para a interface HTTP ou WebSocket:

-   Com os métodos HTTP `GET`e `POST /v1/synthesize`, você especifica um formato usando o cabeçalho da solicitação `Accept`ou o parâmetro de consulta `accept`. Para receber áudio no formato padrão, omita tanto o cabeçalho quanto o parâmetro de consulta. Para obter mais informações, consulte [Sintetizando Texto para Áudio](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingHTTP#synthesize).

    Se você usar o parâmetro de consulta `accept`, a URL-codifique o argumento para o parâmetro. Por exemplo, URL-encode o seguinte argumento

    ```
    áudio / l16; rate=16000 ;endianness=little-endian
    ```
    {: codeblock}

    como

    ```
    áudio %2Fl16%3Brate%3D16000%3Bendianness%3Dlittle-endian
    ```
    {: codeblock}

-   Com a interface WebSocket, você especifica um formato usando o parâmetro `accept`da mensagem de texto que você transmite para iniciar a síntese. Para receber áudio no formato padrão, especifique o valor `*/ *`para o parâmetro. Para obter mais informações, consulte [Enviar texto de entrada](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingWebSocket#WSsend).

## Especificando uma taxa de amostragem
{: #formatRate}

O serviço sempre sintetiza o áudio com uma taxa de amostragem de 22,050 Hz. Para a maioria dos formatos de áudio, o serviço retorna áudio com essa taxa de amostragem padrão. Para alguns formatos, é possível especificar uma taxa de amostragem diferente, incluindo o parâmetro `rate = { rate }`. Para os formatos `audio / l16`e `áudio / mulaw`, você *deve*especificar uma taxa de amostragem.

Quando você especifica uma taxa de amostragem, o serviço resamples o áudio e retorna-o na taxa especificada. Uma taxa de amostragem especificada deve estar no intervalo de 8 kHz a 192 kHz.

A Tabela 2 mostra a taxa de amostragem padrão do áudio que é retornado para cada formato e indica os formatos para os quais você pode ou deve especificar uma taxa de amostragem.

<table style="width:90%">
  <caption>Tabela 2. Taxas de amostragem para formatos de áudio</caption>
  <tr>
    <th style="text-align:left">Formato de áudio</th>
    <th style="text-align:center">Taxa de amostragem padrão</th>
    <th style="text-align:center">Uso do parâmetro <code>rate</code></th>
  </tr>
  <tr>
    <td>
      <code>áudio / básico</code>
    </td>
    <td style="text-align:center">
      8000 Hz
    </td>
    <td style="text-align:center">
      Não permitido
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / flac</code>
    </td>
    <td style="text-align:center">
      22,050 Hz
    </td>
    <td style="text-align:center">
      Opcional
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / l16; taxa = { rate }</code>
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Necessário, por exemplo,<br />
      <code>audio/l16;rate=22050</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/mp3</code><br/>
      <code>audio/mpeg</code>
    </td>
    <td style="text-align:center">
      22,050 Hz
    </td>
    <td style="text-align:center">
      Opcional
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / mulaw; taxa = { rate }</code>
    </td>
    <td style="text-align:center">
      Nenhum
    </td>
    <td style="text-align:center">
      Necessário, por exemplo,<br />
      <code>audio/mulaw;rate=8000</code>
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/ogg</code><br/>
      <code>audio/ogg;codecs=vorbis</code>
    </td>
    <td style="text-align:center">
      22,050 Hz
    </td>
    <td style="text-align:center">
      Opcional
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/ogg;codecs=opus</code>
    </td>
    <td style="text-align:center">
      22.050 Hz [consulte a <strong>Nota</strong>]
    </td>
    <td style="text-align:center">
      Opcional
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / wav</code>
    </td>
    <td style="text-align:center">
      22,050 Hz
    </td>
    <td style="text-align:center">
      Opcional
    </td>
  </tr>
  <tr>
    <td>
      <code>audio/webm</code><br/>
      <code>audio/webm;codecs=opus</code>
    </td>
    <td style="text-align:center">
      48.000 Hz [consulte a <strong>Nota</strong>]
    </td>
    <td style="text-align:center">
      Não permitido
    </td>
  </tr>
  <tr>
    <td>
      <code>áudio / webm; codecs=vorbis</code>
    </td>
    <td style="text-align:center">
      22,050 Hz
    </td>
    <td style="text-align:center">
      Opcional
    </td>
  </tr>
</table>

A maneira mais confiável de identificar a taxa de amostragem para qualquer fluxo de áudio retornado pelo serviço é extrair as informações do fluxo em si. É possível determinar a taxa chamando o método `/v1/synthesize` com algum texto simples (por exemplo, "hello world") e especificando o formato e o codec que você planeja usar. Em seguida, é possível obter o codec e a taxa de amostragem salvando o fluxo de áudio em um arquivo e abrindo-o em um reprodutor de áudio.

O padrão Opus requer que a taxa de amostragem de saída corresponda aos recursos do reprodutor de áudio. Para obter mais informações, consulte a Seção 5.1 do Internet Engineering Task Force (IETF) [Solicitação de comentários (RFC) 7845](http://tools.ietf.org/html/rfc6455){: external}. A tabela indica a taxa de amostragem de saída comum para reprodutores de áudio de software, mas a taxa de amostragem real do áudio varia de acordo com o tempo dentro do fluxo. Conforme mencionado, o serviço sintetiza o áudio de origem em 22.050 Hz.
{: note}

## Reproduzindo um arquivo de áudio
{: #formatsPlay}

Para reproduzir um arquivo de áudio gerado pelo serviço, use uma das ferramentas a seguir:

-   Um navegador da web, como o Google Chrome&trade;, o Firefox&reg; ou o Microsoft&reg; Internet Explorer&reg;.
-   Um reprodutor de áudio, como o Audacity &reg; ([audacityteam.org](http://www.audacityteam.org/){: external}) ou o FFmpeg ([ffmpeg.org](https://www.ffmpeg.org){: external}).
