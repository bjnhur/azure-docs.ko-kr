---
title: 데이터 저장 및 수신 미리 보기-Azure Time Series Insights | Microsoft Docs
description: 데이터 저장 및 수신 Azure Time Series Insights 미리 보기에 대해 알아봅니다.
author: lyrana
ms.author: lyhughes
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/31/2019
ms.custom: seodec18
ms.openlocfilehash: f00529d00312fd6acb045de698590047f991bec7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76714299"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Azure Time Series Insights 미리 보기의 데이터 스토리지 및 수신

이 문서에서는 데이터 저장소에 대 한 업데이트 및 Azure Time Series Insights 미리 보기 수신을 설명 합니다. 기본 스토리지 구조, 파일 형식 및 Time Series ID 속성도 다룹니다. 기본 수신 프로세스, 모범 사례 및 현재 미리 보기 제한 사항에 대해서도 설명 합니다.

## <a name="data-ingress"></a>데이터 수신

Azure Time Series Insights 환경에는 시계열 데이터를 수집, 처리 및 저장 하기 위한 수집 엔진이 포함 됩니다. 환경을 계획할 때에는 들어오는 모든 데이터가 처리 되도록 하기 위해 고려해 야 할 몇 가지 고려 사항이 있습니다. 즉, 들어오는 모든 데이터를 처리 하 고, 수집 대기 시간을 최소화 하 여 수집 대기 시간을 최소화 해야 합니다 (TSI에서 이벤트의 데이터를 읽고 처리 하는 데 소요 되는 시간). 원본). 

Time Series Insights 미리 보기에서 데이터 수신 정책은 데이터를 원본으로 사용할 수 있는 위치와 데이터의 형식을 결정 합니다.

### <a name="ingress-policies"></a>수신 정책

#### <a name="event-sources"></a>이벤트 원본

Time Series Insights 미리 보기는 다음과 같은 이벤트 소스를 지원 합니다.

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Time Series Insights 미리 보기는 인스턴스당 최대 2 개의 이벤트 원본을 지원 합니다.

> [!WARNING] 
> * 미리 보기 환경에 이벤트 원본을 연결할 때 초기 대기 시간이 길어질 수 있습니다. 
> 이벤트 원본 대기 시간은 현재 IoT Hub 또는 이벤트 허브에 있는 이벤트 수에 따라 달라 집니다.
> * 이벤트 원본 데이터가 처음 수집 감소 긴 대기 시간이 발생 합니다. 대기 시간이 계속 되 면 Azure Portal를 통해 지원 티켓을 제출 하 여 문의해 주시기 바랍니다.

#### <a name="supported-data-format-and-types"></a>지원 되는 데이터 형식 및 형식

Azure Time Series Insights Azure IoT Hub 또는 Azure Event Hubs를 통해 전송 되는 UTF8 인코딩 JSON을 지원 합니다. 

다음은 지원 되는 데이터 형식 목록입니다.

| 데이터 형식 | 설명 |
|-----------|------------------|-------------|
| bool      |   두 상태 중 하나를 갖는 데이터 형식 true 또는 false입니다.       |
| dateTime    |   일반적으로 날짜와 시간으로 표시된 시간을 나타냅니다. DateTimes는 ISO 8601 형식 이어야 합니다.      |
| double    |   배정밀도 64 비트 IEEE 754 부동 소수점
| 문자열    |   유니코드 문자로 구성되는 텍스트 값입니다.          |

#### <a name="objects-and-arrays"></a>개체 및 배열

개체 및 배열과 같은 복합 형식을 이벤트 페이로드의 일부로 보낼 수 있지만 데이터를 저장 하면 평면화 프로세스가 수행 됩니다. JSON 이벤트를 셰이핑 하는 방법 및 복합 형식 및 중첩 된 개체 평면화에 대 한 자세한 내용은 [수신 및 쿼리를 위해 json을 모양](./time-series-insights-update-how-to-shape-events.md)설정 하는 방법 페이지를 참조 하세요.


### <a name="ingress-best-practices"></a>수신 모범 사례

다음 모범 사례를 따르는 것이 좋습니다.

* 네트워크 발생 시 수집 대기 시간을 줄이기 위해 동일한 지역에 Time Series Insights 및 IoT Hub 또는 이벤트 허브를 구성 합니다.
* 예상 수집 비율을 계산 하 고 아래에 나열 된 지원 되는 요금 범위 내에 있는지 확인 하 여 규모 요구에 대 한 계획을 수립 합니다.
* [수신 및 쿼리를 위해 json을 shape 하는 방법을](./time-series-insights-update-how-to-shape-events.md)읽어 json 데이터를 최적화 하 고이를 최적화 하는 방법과 미리 보기의 현재 제한 사항을 파악 합니다.

### <a name="ingress-scale-and-limitations-in-preview"></a>미리 보기의 수신 크기 조정 및 제한 사항

#### <a name="per-environment-limitations"></a>환경 제한 사항

일반적으로 수신 속도는 조직에 있는 장치 수, 이벤트 내보내기 빈도 및 각 이벤트의 크기에 대 한 요소로 표시 됩니다.

*  **장치 수** × **이벤트 내보내기 빈도** × **각 이벤트의 크기**입니다.

기본적으로 Time Series Insights 미리 보기는 **TSI 환경 당**최대 1mbps (초당 메가바이트)의 속도로 들어오는 데이터를 수집할 수 있습니다. 사용자의 요구 사항을 충족 하지 않는 경우에는 Azure Portal에 지원 티켓을 제출 하 여 환경에 대해 최대 16mbps를 지원할 수 있습니다.
 
예제 1: Contoso 배송에는 분당 3 회 이벤트를 내보내는 10만 장치가 있습니다. 이벤트 크기는 200 바이트입니다. 4 개의 파티션이 있는 이벤트 허브를 TSI 이벤트 원본으로 사용 하 고 있습니다.
TSI 환경의 수집 비율은 10만 장치 * 200 바이트/이벤트 * (3/60 이벤트/초) = 1 MBps입니다.
파티션당 수집 율은 0.25 MBps입니다.
Contoso 배송료는 미리 보기 크기 제한 내에 있습니다.
 
예 2: Contoso 제 3에는 1 초 마다 이벤트를 내보내는 6만 장치가 있습니다. TSI 이벤트 원본으로 4 개의 IoT Hub 24 파티션 수를 사용 하 고 있습니다. 이벤트 크기는 200 바이트입니다.
환경 수집 율은 2만 장치 * 200 바이트/이벤트 * 1 이벤트/초 = 4 MBps입니다.
파티션당 처리율은 1 MBps입니다.
Contoso는이 규모를 달성할 수 있도록 전용 환경에 대 한 Azure Portal를 통해 TSI에 요청을 제출 해야 합니다.

#### <a name="hub-partitions-and-per-partition-limits"></a>허브 파티션 및 파티션 당 제한

TSI 환경을 계획할 때 TSI에 연결할 이벤트 원본의 구성을 고려 하는 것이 중요 합니다. Azure IoT Hub 및 Event Hubs는 모두 파티션을 활용 하 여 이벤트 처리를 위한 수평 확장을 가능 하 게 합니다.  파티션은 허브에 저장 되는 순서가 지정 된 이벤트 시퀀스입니다. 파티션 수는 IoT 또는 Event Hubs ' 생성 단계 중에 설정 되며 변경할 수 없습니다. 파티션 수를 결정 하는 방법에 대 한 자세한 내용은 Event Hubs ' 필요한 파티션 수는 FAQ를 참조 하세요. IoT Hub를 사용 하는 TSI 환경의 경우 일반적으로 대부분의 IoT 허브에 4 개의 파티션만 필요 합니다. TSI 환경에 대해 새 허브를 만들거나 기존 허브를 사용 하 고 있는지 여부에 관계 없이 파티션 수집 율을 계산 하 여 미리 보기 제한 내에 있는지 확인 해야 합니다. TSI 미리 보기는 현재 **파티션** 제한인 0.5 m b/초를 포함 합니다. 아래 예제를 참조로 사용 하 고 IoT Hub 사용자 인 경우 다음 IoT Hub 관련 고려 사항을 확인 하세요.

#### <a name="iot-hub-specific-considerations"></a>IoT Hub 관련 고려 사항

에서 장치를 만들면 파티션에 할당 된 IoT Hub 파티션 할당이 변경 되지 않습니다. 이렇게 하면 IoT Hub는 이벤트 순서를 보장할 수 있습니다. 그러나이는 특정 시나리오에서 TSI를 다운스트림 판독기로 의미 합니다. 동일한 게이트웨이 장치 ID를 사용 하 여 여러 장치의 메시지를 허브로 전달 하면 동일한 파티션에 도착할 수 있으므로 파티션 크기 조정 제한이 초과 될 수 있습니다. 

**영향**: 단일 파티션이 미리 보기 제한을 초과 하 여 수집 하는 비율이 지속 되는 경우 IoT Hub 데이터 보존 기간을 초과 하기 전에 tsi 판독기가 catch 하지 않을 가능성이 있습니다. 이로 인해 데이터가 손실 될 수 있습니다.

다음 방법을 사용하는 것이 좋습니다. 

* 솔루션을 배포 하기 전에 환경 및 파티션당 수집 요금을 계산 합니다.
* IoT Hub 장치 (및 파티션)가 가장 quantity 확장 가능으로 부하가 분산 되어 있는지 확인 합니다.

> [!WARNING]
> IoT Hub를 이벤트 원본으로 사용 하는 환경의 경우, 사용 중인 허브 장치 수를 사용 하 여 수집 율을 계산 하 여 해당 비율이 미리 보기에서 파티션 제한 당 0.5 MBps 미만으로 떨어질 수 있습니다.

  ![IoT Hub 파티션 다이어그램](media/concepts-ingress-overview/iot-hub-partiton-diagram.png)

처리량 단위 및 파티션에 대 한 자세한 내용은 다음 링크를 참조 하세요.

* [IoT Hub 크기 조정](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [이벤트 허브 크기 조정](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [이벤트 허브 파티션](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>데이터 스토리지

Time Series Insights 미리 보기 종 량 제 SKU 환경을 만들 때 다음 두 가지 Azure 리소스를 만듭니다.

* 웜 스토어 기능을 선택적으로 포함할 수 있는 Time Series Insights 미리 보기 환경입니다.
* 콜드 데이터 저장소에 대 한 Azure Storage 범용 V1 blob 계정.

웜 저장소의 데이터는 [시계열 쿼리](./time-series-insights-update-tsq.md) 및 [Azure Time Series Insights Preview 탐색기](./time-series-insights-update-explorer.md)를 통해서만 사용할 수 있습니다. 

Time Series Insights 미리 보기는 콜드 스토어 데이터를 [Parquet 파일 형식](#parquet-file-format-and-folder-structure)으로 Azure Blob storage에 저장 합니다. Time Series Insights 미리 보기는이 콜드 저장소 데이터를 독점적으로 관리 하지만 표준 Parquet 파일로 직접 읽을 수 있습니다.

> [!WARNING]
> 콜드 스토어 데이터가 있는 Azure Blob storage 계정의 소유자는 계정의 모든 데이터에 대 한 모든 액세스 권한을 갖습니다. 이 액세스에는 쓰기 및 삭제 권한이 포함 됩니다. 데이터 손실이 발생할 수 있기 때문에 미리 보기 쓰기가 Time Series Insights 데이터를 편집 하거나 삭제 하지 마십시오.

### <a name="data-availability"></a>데이터 가용성

최적의 쿼리 성능을 위해 파티션 및 인덱스 데이터를 미리 볼 Time Series Insights. 데이터는 인덱싱된 후 쿼리를 사용할 수 있게 됩니다. 수집 되는 데이터의 양은이 가용성에 영향을 줄 수 있습니다.

> [!IMPORTANT]
> 미리 보기 기간 동안에는 데이터를 사용할 수 있을 때까지 최대 60 초까지 걸릴 수 있습니다. 60 초 보다 긴 대기 시간이 발생 하는 경우 Azure Portal를 통해 지원 티켓을 제출 하세요.

## <a name="azure-storage"></a>Azure Storage

이 섹션에서는 Azure Time Series Insights Preview와 관련 된 Azure Storage 세부 사항을 설명 합니다.

Azure Blob 저장소에 대 한 자세한 설명은 [저장소 blob 소개](../storage/blobs/storage-blobs-introduction.md)를 참조 하세요.

### <a name="your-storage-account"></a>저장소 계정

Time Series Insights 미리 보기 종 량 제 환경을 만들 때 Azure Storage 범용 V1 blob 계정이 장기 콜드 저장소로 생성 됩니다.  

Time Series Insights 미리 보기는 Azure Storage 계정에서 각 이벤트의 복사본을 두 개까지 게시 합니다. 초기 복사본은 수집 시간을 기준으로 정렬 되는 이벤트를 포함 하며 다른 서비스를 사용 하 여 액세스할 수 있습니다. Spark, Hadoop 및 기타 친숙 한 도구를 사용 하 여 원시 Parquet 파일을 처리할 수 있습니다. 

Time Series Insights 미리 보기는 Parquet 파일을 다시 분할 하 여 Time Series Insights 쿼리를 최적화 합니다. 이 다시 분할 데이터 복사본도 저장 됩니다.

공개 미리 보기 중에는 데이터가 Azure Storage 계정에 무기한 저장 됩니다.

#### <a name="writing-and-editing-time-series-insights-blobs"></a>Time Series Insights blob 작성 및 편집

쿼리 성능 및 데이터 가용성을 보장 하려면 미리 보기 Time Series Insights 미리 보기에서 만드는 blob을 편집 하거나 삭제 하지 마십시오.

#### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Time Series Insights 미리 보기에서 데이터 액세스 및 내보내기

Time Series Insights 미리 보기 탐색기에 표시 되는 데이터에 액세스 하 여 다른 서비스와 함께 사용할 수 있습니다. 예를 들어 데이터를 사용 하 여 Power BI에서 보고서를 작성 하거나 Azure Machine Learning Studio를 사용 하 여 기계 학습 모델을 학습할 수 있습니다. 또는 사용자의 데이터를 사용 하 여 Jupyter 노트북에서 변환, 시각화 및 모델링할 수 있습니다.

다음과 같은 세 가지 일반적인 방법으로 데이터에 액세스할 수 있습니다.

* Time Series Insights 미리 보기 탐색기 탐색기에서 CSV 파일로 데이터를 내보낼 수 있습니다. 자세한 내용은 [Time Series Insights Preview 탐색기](./time-series-insights-update-explorer.md)를 참조 하세요.
* 이벤트 가져오기 쿼리를 사용 하 여 Time Series Insights 미리 보기 API를 사용 합니다. 이 API에 대 한 자세한 내용을 보려면 시계열 [쿼리](./time-series-insights-update-tsq.md)를 참조 하세요.
* Azure Storage 계정에서 직접. Time Series Insights 미리 보기 데이터에 액세스 하는 데 사용 중인 계정에 대 한 읽기 권한이 필요 합니다. 자세한 내용은 [저장소 계정 리소스에 대 한 액세스 관리](../storage/blobs/storage-manage-access-to-resources.md)를 참조 하세요.

#### <a name="data-deletion"></a>데이터 삭제

Time Series Insights 미리 보기 파일을 삭제 하지 마세요. Time Series Insights 미리 보기 내 에서만 관련 데이터를 관리 합니다.

### <a name="parquet-file-format-and-folder-structure"></a>Parquet 파일 형식 및 폴더 구조

Parquet는 효율적인 저장소 및 성능을 위해 설계 된 오픈 소스 칼럼 형식 파일 형식입니다. Time Series Insights 미리 보기는 다음과 같은 이유 때문에 Parquet를 사용 합니다. 규모에 따라 쿼리 성능을 위해 시계열 ID로 데이터를 분할 합니다.  

Parquet 파일 형식에 대 한 자세한 내용은 [Parquet 설명서](https://parquet.apache.org/documentation/latest/)를 참조 하세요.

Time Series Insights 미리 보기는 다음과 같이 데이터의 복사본을 저장 합니다.

* 첫 번째 초기 복사본은 수집 시간을 기준으로 분할 되 고 거의 도착 하기 위해 데이터를 저장 합니다. 데이터는 `PT=Time` 폴더에 있습니다.

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* 두 번째 다시 분할 copy는 시계열 Id 그룹으로 분할 되 고 `PT=TsId` 폴더에 상주 합니다.

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

두 경우 모두 time 값은 blob 작성 시간에 해당 합니다. `PT=Time` 폴더의 데이터는 그대로 유지 됩니다. `PT=TsId` 폴더의 데이터는 시간에 따른 쿼리에 최적화 되며 정적으로 유지 되지 않습니다.

> [!NOTE]
> * `<YYYY>`은 네 자리 연도 표현으로 매핑됩니다.
> * `<MM>`는 두 자리 월 표현에 매핑됩니다.
> * `<YYYYMMDDHHMMSSfff>` 4 자리 연도 (`YYYY`), 두 자리 월 (`MM``DD`), 두 자리 시간 (`HH`), 두 자리 분 (`MM`), 두 자리 초 (`SS`) 및 3 자리 밀리초 (`fff`)를 사용 하는 타임 스탬프 표현에 매핑됩니다.

Time Series Insights 미리 보기 이벤트는 다음과 같이 Parquet 파일 내용에 매핑됩니다.

* 각 이벤트는 단일 행에 매핑됩니다.
* 모든 행에는 이벤트 타임 스탬프를 포함 하는 **타임 스탬프** 열이 포함 됩니다. 타임 스탬프 속성은 null 일 수 없습니다. 이벤트 원본에 타임 스탬프 속성이 지정 되지 않은 경우 **이벤트를 큐** 에 넣은 시간으로 기본 설정 됩니다. 타임 스탬프는 항상 UTC로 되어 있습니다.
* 모든 행에는 Time Series Insights 환경을 만들 때 정의 된 시계열 ID 열이 포함 됩니다. 속성 이름에는 `_string` 접미사가 포함 됩니다.
* 원격 분석 데이터로 전송 되는 다른 모든 속성은 속성 형식에 따라 `_string` (문자열), `_bool` (부울), `_datetime` (datetime) 또는 `_double` (double)로 끝나는 열 이름에 매핑됩니다.
* 이 매핑 구성표는 **V = 1**로 참조 되는 파일 형식의 첫 번째 버전에 적용 됩니다. 이 기능이 진화 함에 따라 이름이 증가할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [수신 및 쿼리를 위한 JSON 셰이프를](./time-series-insights-update-how-to-shape-events.md)참조 하세요.

- 새 [데이터 모델링](./time-series-insights-update-tsm.md)에 대해 읽어 보세요.
