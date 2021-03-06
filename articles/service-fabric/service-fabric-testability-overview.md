---
title: 오류 분석 서비스 개요
description: 이 문서는 결함을 유도하고 서비스에 대한 테스트 시나리오를 실행하기 위한 서비스 패브릭의 오류 분석 서비스를 설명합니다.
author: anmolah
ms.topic: conceptual
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: d5c770a4d823ebe9b2700b081c407c54dd1d18a3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75465576"
---
# <a name="introduction-to-the-fault-analysis-service"></a>오류 분석 서비스 소개
오류 분석 서비스는 Microsoft Azure Service Fabric에서 작성된 서비스 테스트를 위해 설계되었습니다. 오류 분석 서비스를 통해 의미 있는 결함을 유도하고 애플리케이션에 대해 전체 테스트 시나리오를 실행할 수 있습니다. 이러한 오류와 시나리오는 다양한 상태를 실행하고 유효성을 검사하며 서비스가 수명 전반에서 일관되게 제어되고 안전한 방식으로 경험할 수 있도록 전환합니다.

작업은 오류를 테스트할 서비스를 대상으로 하는 개별 오류입니다. 서비스 개발자는 이러한 기본 구성 요소를 사용하여 복잡한 시나리오를 작성할 수 있습니다. 예:

* 노드를 다시 시작하여 컴퓨터 또는 VM이 재부팅되는 다양한 상황을 시뮬레이션합니다.
* 상태 저장 서비스의 복제본을 이동하여 부하 분산, 장애 조치(failover) 또는 애플리케이션 업그레이드를 시뮬레이션합니다.
* 상태 저장 서비스에 대해 쿼럼 손실을 호출하여 새 데이터를 허용하기에 충분한 "백업" 또는 "보조" 복제본이 없어서 쓰기 작업을 계속할 수 없는 상황을 만듭니다.
* 상태 저장 서비스에 대해 데이터 손실을 호출하여 모든 메모리 내 상태가 지워진 상황을 만듭니다.

시나리오는 하나 이상의 작업으로 구성되는 복잡한 작업입니다. 오류 분석 서비스는 두 가지 기본 제공 전체 시나리오를 제공합니다.

* 비정상 상황 시나리오
* 장애 조치 시나리오

## <a name="testing-as-a-service"></a>Testing as a service
오류 분석 서비스는 서비스 패브릭 클러스터로 자동 시작되는 서비스 패브릭 시스템 서비스입니다. 이 서비스는 오류 주입, 테스트 시나리오 실행 및 상태 분석을 위한 호스트 역할을 수행합니다. 

![오류 분석 서비스][0]

오류 작업 또는 테스트 시나리오가 시작되면 오류 분석 서비스에 명령을 보내 오류 작업이나 테스트 시나리오를 실행합니다. 오류 분석 서비스는 상태를 저장하므로 안정적으로 오류 및 시나리오를 실행하고 결과에서 유효성을 검사할 수 있습니다. 예를 들어, 오류 분석 서비스를 통해 장기 실행 테스트 시나리오를 안정적으로 실행할 수 있습니다. 또한 테스트가 클러스터 안에서 실행되기 때문에 서비스가 클러스터와 서비스의 상태를 조사하여 결함에 관한 더 심층적인 정보를 제공할 수 있습니다.

## <a name="testing-distributed-systems"></a>분산 시스템 테스트
서비스 패브릭을 사용하면 확장 가능한 분산 애플리케이션의 쓰기 및 관리 작업을 훨씬 간편하게 처리할 수 있습니다. 오류 분석 서비스는 마찬가지로 간편하게 분산된 애플리케이션을 테스트합니다. 테스트 중에 해결해야 하는 세 가지 중요한 문제가 있습니다.

1. 실제 시나리오에서 발생할 수 있는 오류를 시뮬레이션/생성: 서비스 패브릭의 중요한 측면 중 하나는 분산 애플리케이션이 다양한 오류를 복구할 수 있다는 점입니다. 그러나 애플리케이션이 이러한 오류를 복구할 수 있는지 테스트하려면 통제된 테스트 환경에서 실제 오류를 시뮬레이션/생성하는 메커니즘이 필요합니다.
1. 상호 관련된 오류를 생성하는 기능: 네트워크 오류 및 컴퓨터 오류와 같은 시스템의 기본적인 오류를 개별적으로 생성하기란 쉬운 일입니다. 하지만 이러한 개별 오류의 상호 작용으로 인해 실제 환경에서 발생할 수 있는 수많은 시나리오를 생성하기는 어렵습니다.
1. 다양한 개발 및 배포 수준에서 통합되지 않은 환경: 다양한 실패 유형을 테스트할 수 있는 여러 오류 주입 시스템이 있습니다. 그러나 one-box 개발자 시나리오에서 대규모 테스트 환경에서 동일한 테스트를 실행하고 프로덕션 환경에서 테스트하기 위해 사용하도록 전환할 때 이러한 모든 환경은 일정하지 않습니다.

문제를 해결하는 여러 메커니즘이 있지만 one-box 개발자 환경부터 프로덕션 클러스터의 테스트까지 필요한 보증으로 동일한은 기능을 수행하는 시스템은 없습니다 오류 분석 서비스는 애플리케이션 개발자가 비즈니스 논리를 테스트하는 데에 집중할 수 있게 해줍니다. 오류 분석 서비스는 서비스와 기본 분산 시스템 간의 상호 작용을 테스트하는 데 필요한 모든 기능을 제공합니다.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>실제 오류 시나리오 시뮬레이션/생성
분산 시스템의 오류에 대한 견고성을 테스트하려면 오류를 생성하는 메커니즘이 필요합니다. 이론적으로는 노드 다운 같은 오류를 생성하기가 쉬워 보이지만 오류 생성은 서비스 패브릭에서도 아직 해결하지 못한 일관성 문제에서 시작합니다. 예를 들어 한 노드를 종료하려면 다음과 같은 워크플로가 필요합니다.

1. 클라이언트에서 노드 종료 요청을 발급합니다.
1. 올바른 노드에 요청을 보냅니다.
   
    a. 노드를 찾을 수 없으면 요청이 실패합니다.
   
    b. 노드가 발견되면 노드가 종료된 경우에만 요청이 반환됩니다.

테스트 관점에서 오류를 확인하려면 테스트는 이 오류가 유도될 때 오류가 실제로 발생하는지를 알아야 합니다. 서비스 패브릭은 명령이 노드에 도달했을 때 노드가 다운되거나 이미 다운된 상태임을 보증합니다. 어떤 경우든 테스트에서 유효성 검사의 상태와 성공 또는 실패 여부를 올바르게 추론할 수 있습니다. 같은 오류 집합을 수행하도록 서비스 패브릭 외부에서 구현된 시스템은 네트워크, 하드웨어 및 소프트웨어 문제를 과다하게 생성할 수 있고, 이로 인해 이전의 보장을 제공하지 못합니다. 앞에서 언급한 문제가 있으면 서비스 패브릭에서는 클러스터 상태를 다시 구성하여 문제를 해결합니다. 따라서 오류 분석 서비스에서 올바른 보증을 제공할 수 있습니다.

### <a name="generating-required-events-and-scenarios"></a>필요한 이벤트 및 시나리오 생성
실제 오류를 일관적으로 시뮬레이션하기란 시작부터 어려운 일이지만 상호 관련된 오류를 생성하는 기능은 훨씬 더 어렵습니다. 예를 들어 다음 상황이 발생하면 상태 저장 지속형 서비스에서 데이터 손실이 발생합니다.

1. 복제본의 쓰기 쿼럼만 복제본에 catch됩니다. 모든 보조 복제본은 주 복제본 뒤에 지연됩니다.
1. (코드 패키지 또는 노드가 중지되어)복제본이 중지되면 쓰기 쿼럼도 중지됩니다.
1. (디스크 손상 또는 컴퓨터 이미지 다시 설치로 인해)복제본에 대한 데이터가 손실되면 쓰기 쿼럼을 복원할 수 없습니다.

이러한 상호 관련된 오류는 개별 오류만큼 자주는 아니지만 실제 환경에서 분명 발생합니다. 이러한 오류가 발생하기 전에 프로덕션 환경에서 이러한 시나리오를 테스트하는 기능이 매우 중요합니다. 더욱 중요한 점은 (낮에 모든 엔지니어가 작업 중인)제어된 환경에서 프로덕션 워크로드로 이러한 시나리오를 시뮬레이션하는 기능입니다. 오전 2시에 프로덕션 환경에서 처음으로 발생하는 것 보다 훨씬 좋습니다.

### <a name="unified-experience-across-different-environments"></a>여러 환경에서 통합되지 않은 경험
개발 환경용으로 하나, 테스트용으로 하나, 프로덕션용으로 하나를 포함해 세 가지 환경을 만드는 것이 전통적인 관례였습니다. 이 모델은 다음과 같습니다.

1. 개발 환경에서 개별 메서드의 단위 테스트를 허용하는 상태 전환을 생성합니다.
1. 테스트 환경에서 다양한 오류 시나리오를 실행하는 엔드투엔드 테스트를 허용하는 오류를 생성합니다.
1. 프로덕션 환경을 처음 그대로 유지하여 인위적인 오류를 방지하고 사람이 오류에 대해 매우 신속하게 반응하도록 합니다.

서비스 패브릭에서는 오류 분석 서비스를 통해 개발자 환경부터 프로덕션 환경까지 동일한 방법으로 이 문제를 해결할 것을 제안합니다. 두 가지 방법으로 이 작업을 수행할 수 있습니다.

1. 제어된 오류를 유도하려면 one-box 환경부터 프로덕션 클러스터까지 오류 분서 서비스 API를 사용합니다.
1. 클러스터에 자동 오류를 유도하려면 오류 분석 서비스를 사용하여 자동 오류를 생성합니다. 구성을 통해 오류 비율을 제어하여 같은 서비스를 다른 환경에서 다른 방법으로 테스트할 수 있습니다.

서비스 패브릭에서 오류 규모는 환경에 따라 다를 수 있지만 실제 메커니즘은 동일합니다. 따라서 코드-배포 파이프라인 및 기능이 훨씬 빠르게 실제 부하에서 서비스를 테스트할 수 있습니다.

## <a name="using-the-fault-analysis-service"></a>오류 분석 서비스 사용
**C#**

오류 분석 서비스 기능은 Microsoft.ServiceFabric NuGet 패키지의 System.Fabric 네임스페이스에 있습니다. 오류 분석 서비스를 사용하려면 NuGet 패키지를 프로젝트에 참조로 포함하세요.

**PowerShell**

PowerShell을 사용하려면 서비스 패브릭 SDK를 설치해야 합니다. SDK 설치 후에는 사용할 수 있도록 ServiceFabric PowerShell 모듈이 자동으로 로드됩니다.

## <a name="next-steps"></a>다음 단계
진정한 클라우드 규모 서비스를 만들려면 배포 전과 후에 모두 서비스가 실제 오류를 견딜 수 있도록 하는 것이 중요합니다. 오늘날의 서비스 환경에서 신속하게 혁신하고 코드를 프로덕션 환경으로 신속하게 이동하는 기능이 매우 중요합니다. 오류 분석 서비스는 서비스 개발자가 정확하게 작업을 수행하는 데 도움이 됩니다.

기본 제공 [테스트 시나리오](service-fabric-testability-scenarios.md)를 사용하여 애플리케이션 테스트를 시작하거나 오류 분석 서비스에서 제공하는 [오류 작업](service-fabric-testability-actions.md)을 사용하여 자체 테스트 시나리오를 작성합니다.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
