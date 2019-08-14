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

# Présentation pour les développeurs
{: #overview}

Vous pouvez accéder aux fonctions de {{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} via une API REST (Representational State Transfer) HTTP ou une interface WebSocket. Plusieurs kits de développement de logiciels (SDK) sont également disponibles pour simplifier le développement d'application dans différents langages de programmation. Les sections suivantes donnent un aperçu du développement d'application avec le service.
{: shortdesc}

Pour vous authentifier auprès du service {{site.data.keyword.texttospeechshort}}, vous transmettez un jeton d'accès avec chaque demande. {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} est une solution de cloud à service partagé. Vos données d'identification accordent un accès uniquement à vos données, et celles-ci sont isolées des autres utilisateurs.

## Interface HTTP
{: #overview-http}

Pour synthétiser du texte avec l'API HTTP, vous appelez la version `GET` ou `POST` de la méthode `/v1/synthesize` du service. Les deux versions de la méthode offrent des fonctionnalités généralement équivalentes :

-   *Texte en entrée :* vous transmettez le texte en entrée à synthétiser de deux manières différentes :
    -   La méthode `GET /v1/synthesize` accepte le texte d'entrée en tant que paramètre de requête. La taille maximale de la demande est de 8 Ko, ce qui inclut le texte d'entrée, l'URL et les en-têtes.
    -   La méthode `POST /v1/synthesize` accepte le texte d'entrée dans le corps de la demande. La taille maximale de la demande est de 8 Ko pour l'URL et les en-têtes et de 5 Ko pour le texte d'entrée envoyé dans le corps de la demande.

    Vous pouvez transmettre du texte brut du service ou du texte annoté avec le langage SSML (Speech Synthesis Markup Language). SSML est un langage de balisage basé sur XML qui fournit des annotations de texte pour les applications de synthèse vocale telles que le service {{site.data.keyword.texttospeechshort}}.

    Pour plus d'informations, voir [Spécification de texte en entrée](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingHTTP#input).
-   *Voix :* le service accepte le texte et produit de l'audio dans différentes langues, voix et dialectes. Le service offre au moins une voix féminine pour chaque langue. Pour certaines langues, le service offre plusieurs voix, à la fois masculines et féminines. Le service propose également différents dialectes, comme l'anglais britannique et l'anglais américain.

    Vous pouvez utiliser les méthodes `GET /v1/voices` ou `GET /v1/voices/{voice}` du service pour en savoir plus sur les voix prises en charge. Le service synthétise le texte dans la langue de la voix spécifiée. Veillez à faire correspondre la voix au texte en entrée.

    Pour plus d'informations, voir [Langues et voix](/docs/services/text-to-speech-data?topic=text-to-speech-data-voices).
-   *Formats audio :* le service peut produire l'audio dans les formats suivants : Ogg ou Web Media (WebM) avec le codec Opus (par défaut) ou Vorbis, MP3 (Motion Picture Experts Group ou MPEG), WAV (Waveform Audio File Format), FLAC (Free Lossless Audio Codec), Modulation PCM (Pulse-Code Modulation) linéaire 16 bits, mu-law (u-law) 8 bits ou audio de base.

    Pour plus d'informations, voir [Formats audio](/docs/services/text-to-speech-data?topic=text-to-speech-data-audioFormats).

## Interface WebSocket
{: #overview-websocket}

Le service offre une interface WebSocket que vous pouvez utiliser pour synthétiser du texte. L'interface fournit une version unique de la méthode `/v1/synthesize` qui accepte un maximum de 5 Ko de texte en entrée. Vous spécifiez le texte à synthétiser, la voix à utiliser et le format de l'audio. Vous pouvez fournir du texte brut ou du texte annoté avec SSML. Pour plus d'informations, voir [Interface WebSocket](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingWebSocket).

L'interface WebSocket prend en charge l'utilisation de l'élément `<mark>` SSML pour identifier des emplacements spécifiques dans l'audio. Vous pouvez également demander des informations de minutage des mots pour tous les mots du texte en entrée. Pour plus d'informations, voir [Obtention du minutage des mots](/docs/services/text-to-speech-data?topic=text-to-speech-data-timing).

## Interface de personnalisation
{: #overview-customization}

Le service comprend une interface de personnalisation que vous pouvez utiliser pour créer des modèles vocaux personnalisés à utiliser lors de la synthèse vocale. Un modèle vocal personnalisé est un dictionnaire de mots et de leurs traductions pour une langue donnée. Chaque paire mot/traduction d'un modèle indique au service comment prononcer le mot lorsqu'il apparaît dans le texte en entrée.

Vous pouvez utiliser des modèles vocaux personnalisés pour créer des traductions spécifiques à une application pour des mots inhabituels pour lesquels les règles de prononciation habituelles du service risquent de générer des prononciations imparfaites. Vous pouvez définir l'entrée personnalisée pour une paire mot/traduction dans la représentation IPA (International Phonetic Alphabet) standard ou dans la représentation SPR (Symbolic Phonetic Representation) {{site.data.keyword.IBM_notm}} propriétaire.

Par exemple, votre application peut rencontrer régulièrement des termes spéciaux d’origine étrangère, des noms personnels ou géographiques, des abréviations et des acronymes. En utilisant la personnalisation, vous pouvez définir des traductions qui indiquent au service comment vous souhaitez que ces termes soient prononcés. Pour plus d'informations, voir [Compréhension de la personnalisation](/docs/services/text-to-speech-data?topic=text-to-speech-data-customIntro).

L'interface de personnalisation est une version bêta.
{: note}

## Prise en charge de CORS
{: #cors}

Le service prend en charge le partage de ressources d'origine croisée (CORS). A l'aide de CORS, les pages Web peuvent demander des ressources directement à partir d'un domaine étranger. CORS contourne la politique de sécurité de même origine, qui autrement empêche ce type de demande. Etant donné que le service prend en charge CORS, une page Web peut communiquer directement avec le service sans transmettre la demande via le serveur Web qui héberge la page.

Par exemple, une page Web chargée à partir d'un serveur dans {{site.data.keyword.cloud}} peut appeler directement l'API de personnalisation, en contournant le serveur {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [enable-cors.org](https://enable-cors.org/){: external}.
    

## Utilisation de kits de développement de logiciels (SDK)
{: #sdks}

Des SDK sont disponibles pour le service {{site.data.keyword.texttospeechshort}} afin de simplifier le développement d'applications vocales. Les logiciels SDK {{site.data.keyword.ibmwatson}} sont disponibles pour de nombreux langages de programmation et plateformes populaires.

-   Pour obtenir une liste complète des logiciels SDK et des liens vers les logiciels SDK de GitHub, voir [Utilisation de logiciels SDK](/docs/services/watson?topic=watson-using-sdks).
-   Pour des informations détaillées sur toutes les méthodes des logiciels SDK Node, Java&trade;, Python, Ruby et Go pour le service {{site.data.keyword.texttospeechshort}}, voir la [référence d'API](https://{DomainName}/apidocs/text-to-speech-data){: external}.
