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

# Utilisation d'un modèle vocal personnalisé
{: #customUsing}

Une fois que vous avez créé un modèle personnalisé et que vous l'avez rempli avec des entrées personnalisées, vous l'utilisez en transmettant son ID de personnalisation (GUID) avec le paramètre de requête `customization_id` de la méthode HTTP `GET` ou `POST /v1/synthesize` ou de la méthode WebSocket `/v1/synthesize`. Lorsque vous incluez un ID de personnalisation, vous devez appeler une méthode `synthesize` avec les données d'identification de l'instance du service propriétaire du modèle personnalité spécifié.
{: shortdesc}

Les deux premiers exemples génèrent une prononciation personnalisée pour `IEEE` basée sur les entrées du modèle personnalisé indiqué. La prononciation personnalisée est utilisée à la place de la prononciation par défaut des règles de prononciation habituelles du service.

-   Méthode HTTP `GET /v1/synthesize` :

    ```bash
    curl -X GET
    --header "Authorization: Bearer {token}"
    --header "Accept: audio/flac"
    --output ieee.flac
    "{url}/v1/synthesize?text=IEEE&customization_id={customization_id}"
    ```
    {: pre}

-   Méthode HTTP `POST /v1/synthesize` :

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

Le troisième exemple établit une connexion WebSocket avec la méthode `/v1/synthesize` qui utilise le modèle personnalisé indiqué pour synthétiser le texte transmis via la connexion :

```javascript
var my_access_token = '{token}';
var wsURI = '{ws_url}/v1/synthesize'
  + '?access_token=' + my_access_token
  + '&customization_id={customization_id}';
var websocket = new WebSocket(wsURI);
```
{: codeblock}
