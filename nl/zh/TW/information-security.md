---

copyright:
  years: 2019
lastupdated: "2019-06-16"

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

# 資訊安全
{: #information-security}

{{site.data.keyword.IBM}} 致力於為我們的客戶及夥伴提供創新的資料隱私、安全及控管解決方案。
{: shortdesc}

「客戶」應負責自行確保其遵循各種法令規章之規定，包括歐盟的「一般資料保護規範 (General Data Protection Regulation)」。有關任何可能影響客戶業務之相關法令規章之確認與解釋，以及任何客戶為遵循該等法令規章而必須採取之行動，客戶應自行負責向法律顧問取得諮詢意見。
{: important}

本文所述的產品、服務及其他功能並不適合所有客戶情況，並且可用性可能受到限制。{{site.data.keyword.IBM_notm}} 不提供法律、會計或審核建議，也不表示或保證其服務或產品將確保客戶遵守任何法律和法規。

如果您需要針對建立的 {{site.data.keyword.cloud}} {{site.data.keyword.watson}} 資源要求 GDPR 支援：

-   在歐盟 (EU)，請參閱[要求歐盟中所建立 {{site.data.keyword.cloud_notm}} Watson 資源的支援](/docs/services/watson?topic=watson-gdpr-sar#request-EU)。
-   在歐盟以外地區，請參閱[要求歐盟以外資源的支援](/docs/services/watson?topic=watson-gdpr-sar#request-non-EU)。

## 歐盟一般資料保護規範 (GDPR)
{: #gdpr}

{{site.data.keyword.IBM_notm}} 致力於為我們的客戶及夥伴提供創新的資料隱私、安全及控管解決方案，以協助他們邁向 GDPR 法規遵循。

在[這裡](http://www.ibm.com/gdpr){: external}進一步瞭解 {{site.data.keyword.IBM_notm}} 專屬 GDPR 就緒行程以及 GDPR 功能和供應項目，以支援您的規範行程。

## 在文字轉語音服務中標示及刪除資料
{: #gdpr-text-to-speech}

{{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} 可讓您刪除與語音合成要求和自訂語音模型相關聯的所有資料。若要刪除資料，您必須執行下列動作：

1.  使用 `X-Watson-Metadata` 標頭，建立客戶 ID 與藉由要求傳送至服務之資料的關聯；請參閱[指定客戶 ID](#specify-customer-id)。
1.  使用 `DELETE /v1/user_data` 方法，來刪除與所指定客戶 ID 相關聯的所有資料；請參閱[刪除資料](#delete-pi)。

依預設，沒有客戶 ID 與資料相關聯。

實驗性及測試版特性並非旨在與正式作業環境搭配使用，因此，在標示及刪除資料時，不保證如預期運作。在實作需要標示及刪除資料的解決方案時，不應使用實驗性及測試版特性。
{: note}

### 指定客戶 ID
{: #specify-customer-id}

若要建立客戶 ID 與資料的關聯，請在 `X-Watson-Metadata` 標頭中包括傳遞資訊的要求。您可以傳遞 `customer_id={id}` 字串作為標頭的引數。下列範例會建立客戶 ID `my_ID` 與使用 `POST /v1/synthesize` 要求傳遞之資料的關聯：

```bash
curl -X POST
--header "Authorization: Bearer {token}"
--header "X-Watson-Metadata: customer_id=my_ID"
--header "Content-Type: application/json"
--header "Accept: audio/wav"
--data "{\"text\":\"hello world\"}"
--output hello_world.wav
"{url}/v1/synthesize"
```

客戶 ID 可以包括任何字元，但 `;`（分號）和 `=`（等號）除外。為客戶 ID 指定隨機或一般字串；請不要指定個人識別字串，例如電子郵件位址或 Twitter ID。您可以利用不同要求指定不同客戶 ID。您指定的客戶 ID 會與其認證與要求搭配使用的服務實例相關聯；只有該服務實例的認證才能刪除與 ID 相關聯的資料。

使用 `X-Watson-Metadata` 標頭與下列方法搭配：

-   搭配 HTTP 要求：
    -   `POST /v1/synthesize`
    -   `GET /v1/synthesize`

    客戶 ID 會與隨要求一起傳送的資料相關聯。

-   搭配 WebSocket 要求：
    -   `/v1/synthesize`

    您可以使用 `x-watson-metadata` 查詢參數來指定客戶 ID，以建立 ID 與隨要求一起傳送之資料的關聯。您必須將引數以 URL 編碼為查詢參數，例如，`customer_id%3dmy_ID`。

-   搭配要求以將自訂字組新增至自訂語音模型：
    -   `POST /v1/customizations/{customization_id}`
    -   `POST /v1/customizations/{customization_id}/words`
    -   `PUT /v1/customizations/{customization_id}/words/{word}`

    客戶 ID 會與要求所新增或更新之自訂字組相關聯。

### 刪除資料
{: #delete-pi}

若要刪除與客戶 ID 相關聯的所有資料，請使用 `DELETE /v1/user_data` 方法。您可以搭配要求傳遞 `customer_id={id}` 字串作為查詢參數。下列範例會刪除客戶 ID `my_ID` 的所有資料：

```bash
curl -X DELETE
--header "Authorization: Bearer {token}"
"{url}/v1/user_data?customer_id=my_ID"
```

不論使用何種方法新增資訊，`/v1/user_data` 方法都會刪除與所指定客戶 ID 相關聯的所有資料。如果沒有任何資料與客戶 ID 相關聯，則此方法沒有作用。您必須針對用來將客戶 ID 與資料產生關聯的相同服務實例，發出含認證的要求。
