---

copyright:
  years: 2019
lastupdated: "2019-06-22"

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

# 백업 및 복원
{: #backup}

잠재적인 재해로부터 복구하려면 백업을 수행해야 하며 {{site.data.keyword.texttospeechdatafull}} for {{site.data.keyword.icp4dfull}} 복원이 가능하도록 준비해야 합니다. 사용자는 구성, 사용자 정의 및 서비스 사용법에 대해 이해하고 있어야 합니다. 또한 서비스의 인스턴스를 다시 작성하고 데이터를 복원할 준비가 되어 있어야 합니다.
{: shortdesc}

재해 복구는 클라우드 솔루션에서 잠재적인 데이터 손실을 비롯하여 중대한 실패가 발생하는 경우 문제가 될 수 있습니다. 데이터가 손실되는 경우 서비스 인스턴스를 최근의 상태로 리턴하려면 데이터를 복원해야 합니다.

{{site.data.keyword.texttospeechshort}} 서비스의 경우 사용자 정의 음성 모델의 데이터만 클라우드에 저장됩니다. 재해 복구 계획에는 사용자 정의 음성 모델에 대한 파악, 보존 및 이를 복원하기 위한 준비 작업이 포함됩니다.

### 사용자 정의 음성 모델 백업
{: #disaster-recovery-backup}

사용자 정의 음성 모델 및 사용자 정의 항목에 대한 다음 정보를 유지하십시오.

-   모든 사용자 정의 음성 모델 및 정의의 목록입니다. 사용자 정의에 대한 정보를 나열하려면 다음을 수행하십시오.
    -   `GET /v1/customizations` 메소드를 사용하여 모든 사용자 정의 모델에 대한 정보를 나열하십시오. 자세한 정보는 [모든 사용자 정의 모델 조회](/docs/services/text-to-speech-data?topic=text-to-speech-data-customModels#cuModelsQueryAll)를 참조하십시오.
    -   `GET /v1/customizations/{customization_id}` 메소드를 사용하여 지정된 사용자 정의 모델에 대한 정보를 나열하십시오. 자세한 정보는 [사용자 정의 모델 조회](/docs/services/text-to-speech-data?topic=text-to-speech-data-customModels#cuModelsQuery)를 참조하십시오.
-   사용자 정의 음성 모델의 모든 사용자 정의 항목(단어/변환 쌍)에 대한 정보는 다음과 같습니다.
    -   `GET /v1/customizations/{customization_id}/words` 메소드를 사용하여 사용자 정의 모델에서 모든 단어/변환 쌍에 대한 정보를 나열합니다. 자세한 정보는 [사용자 정의 모델에서 모든 단어 조회](/docs/services/text-to-speech-data?topic=text-to-speech-data-customWords#cuWordsQueryModel)를 참조하십시오.
    -   `GET /v1/customizations/{customization_id}/words/{word}` 메소드를 사용하여 사용자 정의 모델에서 지정된 단어/변환 쌍에 대한 정보를 나열합니다. 자세한 정보는 [사용자 정의 모델에서 하나의 단어 조회](/docs/services/text-to-speech-data?topic=text-to-speech-data-customWords#cuWordQueryModel)를 참조하십시오.

실패 발생 시 사용자 정의 음성 모델을 다시 작성하는 데 사용할 수 있는 형식으로 이 정보를 보존하는 것이 좋습니다. 사용자 정의 모델 및 사용자 정의 항목에 대한 정보를 계속해서 유지보수하고 다음 절에 나열된 호출을 미리 준비하면 가능한 빨리 복구할 수 있습니다.

### 사용자 정의 음성 모델 복원
{: #disaster-recovery-restore}

재해를 복구해야 하는 경우 백업 정보를 사용하여 사용자 정의 음성 모델 및 사용자 정의 항목을 다시 작성할 수 있습니다.

1.  사용자 정의 음성 모델을 다시 작성하려면 `POST /v1/customizations` 메소드를 사용하십시오. 자세한 정보는 [사용자 정의 모델 작성](/docs/services/text-to-speech-data?topic=text-to-speech-data-customModels#cuModelsCreate)을 참조하십시오.
1.  여러 단어/변환 쌍을 사용자 정의 음성 모델에 추가하려면 `POST /v1/customizations/{customization_id}/words` 메소드를 사용하십시오. 자세한 정보는 [사용자 정의 모델에 여러 단어 추가](/docs/services/text-to-speech-data?topic=text-to-speech-data-customWords#cuWordsAdd)를 참조하십시오.
1.  하나의 단어/변환 쌍을 사용자 정의 음성 모델에 추가하려면 `POST /v1/customizations/{customization_id}/words/{word}` 메소드를 사용하십시오. 자세한 정보는 [사용자 정의 모델에 하나의 단어 추가](/docs/services/text-to-speech-data?topic=text-to-speech-data-customWords#cuWordAdd)를 참조하십시오.

모든 사용자 정의 항목을 한 번에 추가하거나 그룹별로 한 번에 하나씩 추가할 수 있습니다.
