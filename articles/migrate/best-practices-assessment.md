---
title: Azure Migrate Server 평가의 평가 모범 사례
description: Azure Migrate Server 평가를 사용 하 여 평가를 만들기 위한 팁입니다.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: de6953b6648613595bc9975b17941b3a453a6d60
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185974"
---
# <a name="best-practices-for-creating-assessments"></a>평가 만들기에 대 한 모범 사례

[Azure Migrate](migrate-overview.md)는 앱, 인프라 및 워크로드를 검색, 평가 및 Microsoft Azure로 마이그레이션하는 데 도움이 되는 도구의 허브를 제공합니다. 허브에는 Azure Migrate 도구와 타사 ISV(독립 소프트웨어 공급업체) 제품이 포함되어 있습니다.

이 문서에서는 Azure Migrate 서버 평가 도구를 사용 하 여 평가를 만들 때의 모범 사례를 요약 합니다.

## <a name="about-assessments"></a>평가 정보

Azure Migrate Server 평가를 사용 하 여 만든 평가는 데이터의 특정 시점 스냅숏입니다. Azure Migrate에는 두 가지 유형의 평가가 있습니다.

**평가 유형** | **세부 정보** | **데이터**
--- | --- | ---
**성능 기반** | 수집 된 성능 데이터를 기반으로 권장 사항을 만드는 평가 | VM 크기 권장 사항은 CPU 및 메모리 사용률 데이터를 기반으로 합니다.<br/><br/> 디스크 유형 권장 사항 (표준 HDD/SSD 또는 프리미엄 관리 디스크)은 온-프레미스 디스크의 IOPS 및 처리량을 기반으로 합니다.
**온-프레미스 인 경우** | 권장 사항을 적용 하기 위해 성능 데이터를 사용 하지 않는 평가 | VM 크기 권장 사항은 온-프레미스 VM 크기를 기반으로 합니다.<br/><br> 권장 디스크 유형은 평가의 저장소 유형 설정에서 선택한 항목을 기반으로 합니다.

### <a name="example"></a>예
예를 들어 4 개 코어를 사용 하는 온-프레미스 VM의 사용률은 20%이 고 메모리는 8gb 이며 10% 사용률 이면 평가는 다음과 같습니다.

- **성능 기반 평가**:
    - 코어 (4 x 0.20 = 0.8) 및 메모리 (8gb x 0.10 = 0.8) 사용률을 기준으로 유효한 코어 및 메모리를 식별 합니다.
    - 평가 속성에 지정 된 편안한 요소를 적용 하 여 (예를 들어 1.3 x) 크기 조정에 사용할 값을 가져옵니다. 
    - ~ 1.04 코어 (0.8 x 1.3) 및 ~ 1.04 GB (0.8 x 1.3) 메모리를 지원할 수 있는 Azure의 가장 가까운 VM 크기를 권장 합니다.

- **As (온-프레미스) 평가**:
    -  4 개의 코어가 있는 VM을 권장 합니다. 8gb의 메모리

## <a name="best-practices-for-creating-assessments"></a>평가 만들기에 대 한 모범 사례

Azure Migrate 어플라이언스는 온-프레미스 환경을 지속적으로 프로 파일링 하 고 메타 데이터 및 성능 데이터를 Azure로 전송 합니다. 어플라이언스를 사용 하 여 검색 된 서버를 평가 하기 위한 다음 모범 사례를 따르세요.

- 있는 **그대로 만들기 평가**: Azure Migrate 포털에서 컴퓨터가 표시 되 면 즉시 평가를 만들 수 있습니다.
- **성능 기반 평가 만들기**: 검색을 설정한 후에는 성능 기반 평가를 실행 하기 전에 적어도 하루 동안 대기 하는 것이 좋습니다.
    - 성능 데이터를 수집 하는 데 시간이 걸립니다. 적어도 하루를 대기 하면 평가를 실행 하기 전에 성능 데이터 요소가 충분 한지 확인 합니다.
    - 성능 기반 평가를 실행 하는 경우 평가 기간에 대 한 환경을 프로 파일링 해야 합니다. 예를 들어 성능 기간이 1 주일로 설정 된 평가를 만들 경우 검색을 시작한 후 적어도 1 주일 후에 모든 데이터 요소가 수집 될 때까지 기다려야 합니다. 그렇지 않으면 평가는 별 5 개 등급을 얻지 못합니다.
- **평가 다시 계산**: 평가는 지정 시간 스냅숏 이므로 최신 데이터로 자동으로 업데이트 되지 않습니다. 최신 데이터로 평가를 업데이트 하려면 다시 계산 해야 합니다.

를 통해 Azure Migrate로 가져온 서버를 평가 하기 위한 다음 모범 사례를 따르세요. CSV 파일:

- 있는 **그대로 만들기 평가**: Azure Migrate 포털에서 컴퓨터가 표시 되 면 즉시 평가를 만들 수 있습니다.
- **성능 기반 평가 만들기**: 이렇게 하면 특히 온-프레미스에서 과도 하 게 프로 비전 된 서버 용량을 사용 하는 경우 보다 비용을 더 효율적으로 예측할 수 있습니다. 그러나 성능 기반 평가의 정확도는 서버에 대해 사용자가 지정한 성능 데이터에 따라 달라 집니다. 
- **평가 다시 계산**: 평가는 지정 시간 스냅숏 이므로 최신 데이터로 자동으로 업데이트 되지 않습니다. 가져온 최신 데이터로 평가를 업데이트 하려면 다시 계산 해야 합니다.

## <a name="best-practices-for-confidence-ratings"></a>신뢰 등급에 대 한 모범 사례

성능 기반 평가를 실행 하는 경우 1 별 (최저)에서 5 별 (가장 높음) 까지의 신뢰 등급이 평가로 표시 됩니다. 신뢰 등급을 효과적으로 사용 하려면 다음을 수행 합니다.
- Azure Migrate 서버 평가에는 VM CPU/메모리에 대 한 사용률 데이터가 필요 합니다.
- 온-프레미스 VM에 연결 된 각 디스크에 대해 읽기/쓰기 IOPS/처리량 데이터가 필요 합니다.
- VM에 연결 된 각 네트워크 어댑터에 대 한 네트워크 연결/출력 데이터가 필요 합니다.

선택한 기간에 사용할 수 있는 데이터 요소의 백분율에 따라 평가의 신뢰 등급이 다음 표에 요약 되어 제공 됩니다.

   **데이터 요소 가용성** | **신뢰 등급**
   --- | ---
   0%-20% | 별 1개
   21%-40% | 별 2개
   41%-60% | 별 3개
   61%-80% | 별 4개
   81%-100% | 별 5개


## <a name="common-assessment-issues"></a>일반적인 평가 문제

평가에 영향을 주는 몇 가지 일반적인 환경 문제를 해결 하는 방법은 다음과 같습니다.

###  <a name="out-of-sync-assessments"></a>동기화 되지 않은 평가

평가를 만든 후 그룹에서 컴퓨터를 추가 하거나 제거 하는 경우 사용자가 만든 평가가 **동기화**되지 않은 것으로 표시 됩니다. 평가를 다시 실행 하 여 그룹 변경 내용을 반영 합니다 (**다시 계산**).

### <a name="outdated-assessments"></a>오래 된 평가

평가 된 그룹에 있는 Vm에 대 한 온-프레미스 변경 내용이 있는 경우 평가는 **오래**된 것으로 표시 됩니다. 변경 내용을 반영 하려면 평가를 다시 실행 합니다.

### <a name="low-confidence-rating"></a>낮은 신뢰 등급

평가에는 여러 가지 이유로 인해 모든 데이터 요소가 포함 되지 않을 수 있습니다.

- 평가를 작성하는 기간 동안 환경을 프로파일링하지 않았습니다. 예를 들어 성능 기간을 1 주일으로 설정 하 여 *성능 기반 평가* 를 만드는 경우 모든 데이터 요소에 대 한 검색을 시작 하 고 나 서 적어도 한 주 동안 기다려야 합니다. 언제 든 지 **다시 계산** 을 클릭 하 여 해당 하는 최신 신뢰 등급을 확인할 수 있습니다. 신뢰 등급은 *성능 기반* 평가를 만들 때만 적용 됩니다.

- 평가 계산 기간에 일부 VM이 종료되었습니다. 평가 기간 중 일부 VM이 꺼지면 Server Assessment에서 해당 기간의 성능 데이터를 수집할 수 없습니다.

- Server Assessment에서 검색이 시작된 후 VM 몇 개가 생성되었습니다. 예를 들어 마지막 1달의 성능 기록에 대한 평가를 만들려고 하는데, 일부 VM이 불과 일주일 전에 환경에서 생성되었습니다. 이 경우 새 VM의 성능 데이터를 전체 기간에 사용할 수 없으며 신뢰 등급이 낮아집니다.


## <a name="next-steps"></a>다음 단계

- 평가를 계산 하는 방법을 [알아봅니다](concepts-assessment-calculation.md) .
- 평가를 사용자 지정 하는 방법을 [알아봅니다](how-to-modify-assessment.md) .
