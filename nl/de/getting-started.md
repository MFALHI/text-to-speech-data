---

copyright:
  years: 2019
lastupdated: "2019-07-30"

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
{:go: .ph data-hd-programlang='go'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}
{:url: data-credential-placeholder='url'}
{:hide-dashboard: .hide-dashboard}

# Lernprogramm 'Einführung'
{: #gettingStarted}

{{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} konvertiert geschriebenen Text in gut verständliche Sprache, damit Sprachsynthesefunktionen für Anwendungen zur Verfügung stehen. Das vorliegende auf Curl basierende Lernprogramm unterstützt Sie beim schnellen Einstieg in diesen Service. Die Beispiele demonstrieren, wie Sie durch den Aufruf der vom Service bereitgestellten Methoden `POST` und `GET /v1/synthesize` einen Audiodatenstrom anfordern.
{: shortdesc}

## Vorbereitende Schritte
{: #before-you-begin}

Damit Sie {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} verwenden können, müssen Sie zunächst die folgenden Schritte ausführen:

1.  Stellen Sie eine Instanz von {{site.data.keyword.texttospeechshort}} for {{site.data.keyword.icp4dfull_notm}} bereit. Weitere Informationen zur Bereitstellung finden Sie in [Service installieren](/docs/services/text-to-speech-data?topic=text-to-speech-data-install).
1.  Wählen Sie im Menü des {{site.data.keyword.icp4dfull_notm}}-Web-Clients **Eigene Instanzen** aus.
1.  Klicken Sie auf die {{site.data.keyword.texttospeechshort}}-Instanz, um die Übersichtsseite zu öffnen. Kopieren Sie die Berechtigungsnachweiswerte für `{token}` und `{URL}`. 

### curl-Beispiele verwenden
{: #getting-started-curl}

In diesem Lernprogramm wird der Befehl `curl` verwendet, um Methoden der HTTP-Schnittstelle des Service aufzurufen. Stellen Sie sicher, dass der Befehl `curl` auf Ihrem System installiert ist.

1.  Um zu testen, ob `curl` installiert ist, führen Sie den folgenden Befehl in der Befehlszeile aus. Wenn in der Ausgabe die `curl`-Version aufgelistet wird, die Secure Sockets Layer (SSL) unterstützt, sind Sie für das Lernprogramm bereit.

    ```bash
    curl -V
    ```
    {: pre}

1.  Falls erforderlich, installieren Sie die Version von `curl` mit aktivierter SSL für Ihr Betriebssystem über [curl.haxx.se](https://curl.haxx.se/){: external}.

Lassen Sie die in den Beispielen angegebenen geschweiften Klammern weg. Diese stellen Variablenwerte dar.
{: tip}

## Schritt 1: Synthetische Sprachausgabe aus Text in amerikanischem Englisch erstellen
{: #synthesizeEnglish}

Die folgenden Befehle verwenden die Methode `POST /v1/synthesize`, um aus Eingabe in amerikanischem Englisch synthetisch Audiodateien in zwei unterschiedlichen Formaten zu erstellen. Beide Anforderungen verwenden die Standardstimme für amerikanisches Englisch `en-US_MichaelVoice`.

1.  Geben Sie den folgenden Befehl aus, um für die Zeichenfolge 'hello world' synthetisch Sprachausgabe zu erstellen und eine WAV-Datei namens `hello_world.wav` zu erzeugen.
    -   Ersetzen Sie `{token}` durch das Zugriffstoken für Ihre Serviceinstanz.
    -   Ersetzen Sie `{url}` durch die URL für Ihre Serviceinstanz.

    *Windows-Benutzer* ersetzen den umgekehrten Schrägstrich (``\`) am Ende jeder Zeile mit einem Winkelzeichen (``^`). Stellen Sie sicher, dass keine nachgestellten Leerzeichen vorhanden sind.
    {: tip}

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: application/json" \
    --header "Accept: audio/wav" \
    --data "{\"text\":\"hello world\"}" \
    --output hello_world.wav \
    "{url}/v1/synthesize"
    ```
    {: pre}

1.  Geben Sie den folgenden Befehl aus, um synthetisch Sprachausgabe aus demselben Text zu erstellen, jedoch eine Datei im Standardformat 'Ogg' namens `hello_world.ogg` zu erzeugen.
    -   Ersetzen Sie `{token}` durch das Zugriffstoken für Ihre Serviceinstanz.
    -   Ersetzen Sie `{url}` durch die URL für Ihre Serviceinstanz.

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --header "Content-Type: application/json" \
    --data "{\"text\":\"hello world\"}" \
    --output hello_world.ogg \
    "{url}/v1/synthesize"
    ```
    {: pre}

Zur Wiedergabe der Audiodateien, die durch die Beispiele erzeugt werden, können Sie einen Browser oder andere Tools verwenden. Weitere Informationen finden Sie im Abschnitt [Audiodatei wiedergeben](/docs/services/text-to-speech-data?topic=text-to-speech-data-audioFormats#formatsPlay).
{: note}

## Schritt 2: Sprachausgabe synthetisch aus Text in Spanisch erstellen
{: #synthesizeSpanish}

Der folgende Befehl verwendet die Methode `GET /v1/synthesize`, um aus Eingabe in Spanisch synthetisch eine Audiodatei zu erstellen.

1.  Geben Sie den folgenden Befehl aus, um synthetisch eine Sprachausgabe für die Zeichenfolge 'hola mundo' zu erstellen und eine WAV-Datei namens `hola_mundo.wav` zu erzeugen. Der Eingabetext ist URL-codiert. Die Methode beinhaltet die Abfrageparameter `accept` zur Angabe des Audioformats und `voice` zur Angabe der Stimme für Spanisch namens `es-ES_EnriqueV3Voice`.
    -   Ersetzen Sie `{token}` durch das Zugriffstoken für Ihre Serviceinstanz.
    -   Ersetzen Sie `{url}` durch die URL für Ihre Serviceinstanz.

    ```bash
    curl -X POST \
    --header "Authorization: Bearer {token}" \
    --output hola_mundo.wav \
    "{url}/v1/synthesize?accept=audio%2Fwav&text=hola%20mundo&voice=es-ES_EnriqueV3Voice"
    ```
    {: pre}

## Nächste Schritte

-   Informieren Sie sich im Abschnitt [HTTP-Schnittstelle](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingHTTP) über die HTTP-Schnittstelle des Service.
-   Informieren Sie sich im Abschnitt [WebSocket-Schnittstelle](/docs/services/text-to-speech-data?topic=text-to-speech-data-usingWebSocket) über die WebSocket-Schnittstelle des Service.
-   Ausführliche Informationen zu allen Methoden der Serviceschnittstellen sind in der [API-Referenz](https://{DomainName}/apidocs/text-to-speech-data){: external} enthalten.
