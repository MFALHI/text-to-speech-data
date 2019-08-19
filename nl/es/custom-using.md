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

# Utilización de un modelo de voz personalizado
{: #customUsing}

Una vez que ha creado un modelo personalizado y lo ha cumplimentado con entradas personalizadas, lo puede utilizar pasando el ID de personalización (GUID) con el parámetro de consulta `customization_id` del método HTTP `GET` o `POST /v1/synthesize` o del método WebSocket `/v1/synthesize` method. Cuando incluya un ID de personalización, debe llamar al método `synthesize` con credenciales para la instancia del servicio que posee el modelo personalizado especificado.
{: shortdesc}

En los dos primeros ejemplos se genera una pronunciación personalizada para `IEEE` basada en las entradas del modelo personalizado indicado. Se utiliza la pronunciación personalizada en lugar de la pronunciación predeterminada a partir de las reglas de pronunciación regular del servicio.

-   El método HTTP `GET /v1/synthesize`:

    ```bash
    curl -X GET
    --header "Authorization: Bearer {token}"
    --header "Accept: audio/flac"
    --output ieee.flac
    "{url}/v1/synthesize?text=IEEE&customization_id={customization_id}"
    ```
    {: pre}

-   El método HTTP `POST /v1/synthesize`:

    ```bash
    curl -X POST
    --header "Authorization: Bearer {token}"
    --header "Content-Type: application/json"
    --header "Accept: audio/flac"
    --data "{\"text\":\"IEEE\"}"
    --output ieee.flac
    "{url}/v1/synthesize?customization_id={customization_id}"
    ```
    {: pre}

En el tercer ejemplo se establece una conexión de WebSocket con el método `/v1/synthesize` que utiliza el modelo personalizado indicado para sintetizar el texto pasado a través de la conexión:

```javascript
var my_access_token = '{token}';
var wsURI = '{ws_url}/v1/synthesize'
  + '?access_token=' + my_access_token
  + '&customization_id={customization_id}';
var websocket = new WebSocket(wsURI);
```
{: codeblock}
