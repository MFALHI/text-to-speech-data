---

copyright:
  years: 2019
lastupdated: "2019-07-06"

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

# Visión general para desarrolladores
{: #overview}

Puede acceder a las prestaciones de {{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} a través de una API REST (Representational State Transfer) HTTP o de una interfaz de WebSocket. También hay varios SDK disponibles para simplificar el desarrollo de aplicaciones en diversos lenguajes de programación. En las secciones siguientes se proporciona una visión general del desarrollo de aplicaciones con el servicio.
{: shortdesc}

Se efectúa la autenticación en el servicio de {{site.data.keyword.texttospeechshort}} pasando un señal de acceso con cada solicitud. {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} es una solución en la nube de multiarrendatario. Sus credenciales proporcionan acceso solamente a los datos y los datos están aislados de otros usuarios.

## Interfaz HTTP
{: #overview-http}

Para sintetizar texto con la API HTTP, puede llamar a la versión `GET` o `POST` del método `/v1/synthesize` del servicio. Las dos versiones del método ofrecen una funcionalidad generalmente equivalente:

-   *Texto de entrada:* El texto de entrada a sintetizar se puede pasar de dos formas distintas:
    -   El método `GET /v1/synthesize` acepta el texto de entrada como un parámetro de consulta. El tamaño máximo de la solicitud es de 8 KB, incluyendo el texto de entrada y el URL y las cabeceras.
    -   El método `POST /v1/synthesize` acepta el texto de entrada en el cuerpo de la solicitud. El tamaño máximo de la solicitud es de 8 KB para el URL y las cabeceras y de 5 KB para el texto de entrada que se envía dentro del cuerpo de la solicitud.

    Puede pasar puede pasar al servicio texto sin formato o texto anotado con SSML (Speech Synthesis Markup Language). SSML es un lenguaje de códigos basado en XML que proporciona anotaciones de texto para aplicaciones de síntesis de voz, como por ejemplo el servicio {{site.data.keyword.texttospeechshort}}.

    Para obtener más información, consulte [Especificar el texto de entrada](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingHTTP#input).
-   *Voces:* El servicio acepta texto y produce audio en varios idiomas, voces y dialectos. El servicio ofrece al menos una voz femenina para cada idioma. Para algunos idiomas, el servicio ofrece varias voces, tanto masculinas como femeninas. El servicio también ofrece varios dialectos como, por ejemplo, el inglés americano y británico.

    Puede utilizar los métodos `GET /v1/voices` o `GET /v1/voices/{voice}` del servicio para obtener más información acerca de las voces soportadas. El servicio sintetiza el texto en el idioma de la voz especificada. Asegúrese de que la voz seleccionada coincida en idioma con el texto de entrada.

    Para obtener más información, consulte [Idiomas y voces](/docs/services/text-to-speech-data?topic=text-to-speech-data-voices).
-   *Formatos de audio::* El servicio puede producir audio en los formatos siguientes: formato Ogg o Web Media (WebM) con el códec Opus (predeterminado) o Vorbis, formato MP3 (Motion Picture Experts Group, o MPEG), formato Waveform Audio File (WAV), formato Free Lossless Audio Codec (FLAC), Pulse-Code Modulation (PCM) Lineal de 16 bits, mu-law (u-law) de 8 bits, o audio básico.

    Para obtener más información, consulte [Formatos de audio](/docs/services/text-to-speech-data?topic=text-to-speech-data-audioFormats).

## Interfaz WebSocket
{: #overview-websocket}

El servicio ofrece una interfaz WebSocket que se puede utilizar para sintetizar texto. La interfaz proporciona una única versión del método `/v1/synthesize` que acepta un máximo de 5 KB de texto de entrada. Se debe especificar el texto a sintetizar, la voz a utilizar y el formato de audio. Se puede proporcionar texto sin formato o texto anotado con SSML. Para obtener más información, consulte [La interfaz WebSocket](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingWebSocket).

La interfaz WebSocket admite el uso del elemento SSML `<mark>` para identificar ubicaciones específicas en el audio. También puede solicitar información de temporización de palabras para todas las palabras del texto de entrada. Para obtener más información, consulte [Obtener temporizaciones de palabras](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing).

## Interfaz de personalización
{: #overview-customization}

El servicio incluye una interfaz de personalización que se puede utilizar para crear modelos de voz personalizados para su uso durante la síntesis de voz. Un modelo de voz personalizado es un diccionario de palabras y sus conversiones para un idioma específico. Cada par de palabra/conversión de un modelo indica al servicio cómo pronunciar la palabra cuando aparece en el texto de entrada.

Puede utilizar modelos de voz personalizados para crear conversiones específicas de la aplicación para las palabras inusuales para las que las reglas de pronunciación regular del servicio pueden producir pronunciaciones imperfectas. Puede definir la entrada personalizada para un par de palabra/conversión en la representación estándar IPA (International Phonetic Alphabet) o en la representación SPR (Symbolic Phonetic Representation), propiedad de {{site.data.keyword.IBM_notm}}.

Por ejemplo, la aplicación puede encontrarse rutinariamente con términos especiales de origen extranjero, nombres de persona o geográficos, o abreviaturas y acrónimos. Utilizando la personalización, puede definir conversiones que indiquen al servicio cómo desea que se pronuncien dichos términos. Para obtener más información, consulte [Comprender la personalización](/docs/services/text-to-speech-data?topic=text-to-speech-data-customIntro).

La interfaz de personalización es un release beta.
{: note}

## Soporte de CORS
{: #cors}

El servicio da soporte a CORS (Cross-Origin Resource Sharing). Utilizando CORS, las páginas web pueden solicitar recursos directamente de un dominio foráneo. CORS omite la política de seguridad del mismo origen, que de lo contrario impide tales solicitudes. Como el servicio da soporte a CORS, una página web puede comunicarse directamente con el servicio sin pasar la solicitud a través del servidor web que aloja la página.

Por ejemplo, una página web que se carga desde un servidor en {{site.data.keyword.cloud}} puede llamar directamente a la API de personalización, omitiendo el servidor de {{site.data.keyword.cloud_notm}}. Para obtener más información, consulte [enable-cors.org](https://enable-cors.org/){: external}.

## Utilización de los SDK (Software Development Kits)
{: #sdks}

Hay SDK disponibles para el servicio de {{site.data.keyword.texttospeechshort}} para simplificar el desarrollo de las aplicaciones de voz. Hay SDK de {{site.data.keyword.ibmwatson}} disponibles para muchos lenguajes de programación y plataformas ampliamente utilizados.

-   Para obtener una lista completa de los SDK y los enlaces a los SDK de GitHub, consulte [Utilización de SDK](/docs/services/watson?topic=watson-using-sdks).
-   Para obtener información detallada sobre todos los métodos de los SDK de Node, Java&trade;, Python, Ruby y Go para el servicio {{site.data.keyword.texttospeechshort}}, consulte la publicación [Consulta de API](https://{DomainName}/apidocs/text-to-speech-data){: external}.
