---
title: Azure Site Recovery를 사용 하 여 Azure Vm을 주 지역으로 다시 보호 Microsoft Docs
description: Azure Site Recovery를 사용 하 여 장애 조치 (failover) 후 보조 지역에서 주 지역으로 Azure Vm을 다시 보호 하는 방법을 설명 합니다.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 955e1b84f897a5eb877033e0a58b8d661f143a14
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73954181"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>주 지역으로 장애 조치(failover)된 Azure VM 다시 보호


[Azure Site Recovery](site-recovery-failover.md)를 사용하여 한 지역에서 다른 지역으로 Azure VM을 [장애 조치](site-recovery-overview.md)(failover)할 경우 VM은 보조 지역에서 보호되지 않는 상태로 부팅됩니다. VM을 주 지역으로 장애 복구(failback)하려면 다음을 수행해야 합니다.

- VM이 주 지역으로 복제를 시작하도록 보조 지역의 VM을 다시 보호합니다.
- 다시 보호를 완료하고 VM이 복제되면 보조 지역에서 주 지역으로 VM을 장애 조치(failover)할 수 있습니다.

## <a name="prerequisites"></a>선행 조건
1. 주 지역에서 보조 지역으로 VM 장애 조치(failover)를 커밋해야 합니다.
2. 주 대상 사이트가 사용 가능하고 해당 지역에서 리소스를 만들거나 액세스할 수 있어야 합니다.

## <a name="reprotect-a-vm"></a>VM 다시 보호

1. **자격 증명 모음** > **복제된 항목**에서 장애 조치(failover)된 VM을 마우스 오른쪽 단추로 클릭하고 **다시 보호**를 선택합니다. 다시 보호 방향이 보조에서 주로 표시됩니다.

   ![다시 보호](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. 리소스 그룹, 네트워크, 스토리지 및 가용성 집합을 검토합니다. 그런 후 **OK**를 클릭합니다. 신규로 표시된 리소스가 있는 경우 이러한 리소스는 다시 보호 프로세스의 일부로 만들어집니다.
3. 다시 보호 작업은 최신 데이터로 대상 사이트를 시드합니다. 완료되면 델타 복제가 수행됩니다. 그런 다음 주 사이트로 다시 장애 조치(failover)할 수 있습니다. 사용자 지정 옵션을 사용하여 다시 보호 중에 사용할 네트워크나 스토리지 계정을 선택할 수 있습니다.

   ![옵션 사용자 지정](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>다시 보호 설정 사용자 지정

다시 보호하는 동안 대상 VM의 다음 속성을 사용자 지정할 수 있습니다.

![사용자 지정](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|속성 |참고 사항  |
|---------|---------|
|대상 리소스 그룹     | VM이 만들어진 대상 리소스 그룹을 수정합니다. 다시 보호의 일부로 대상 VM이 삭제됩니다. 장애 조치(failover) 후 VM을 만들 새 리소스 그룹을 선택할 수 있습니다.        |
|대상 가상 네트워크     | 다시 보호 작업 동안 대상 네트워크를 변경할 수 없습니다. 네트워크를 변경하려면 네트워크 매핑을 다시 실행합니다.         |
|대상 스토리지(보조 VM이 관리 디스크를 사용하지 않음)     | 장애 조치(failover) 후 VM이 사용하는 스토리지 계정을 변경할 수 있습니다.         |
|복제본 관리 디스크(보조 VM이 관리 디스크를 사용함)    | Site Recovery는 보조 VM의 관리 디스크를 미러링하는 주 지역에서 관리 디스크의 복제본을 만듭니다.         |
|캐시 스토리지     | 복제하는 동안 사용할 캐시 스토리지 계정을 지정할 수 있습니다. 기본적으로 캐시 스토리지 계정이 없는 경우 새 캐시 스토리지 계정이 만들어집니다.         |
|가용성 집합     |보조 지역의 VM이 가용성 집합의 일부인 경우 주 지역의 대상 VM에 대한 가용성 집합을 선택할 수 있습니다. 기본적으로 Site Recovery는 주 지역에서 기존 가용성 집합을 찾고 사용합니다. 사용자 지정하는 동안 새 가용성 집합을 지정할 수 있습니다.         |


### <a name="what-happens-during-reprotection"></a>다시 보호하는 동안 어떻게 되나요?

기본적으로 다음이 발생합니다.

1. 장애 조치된 VM이 실행 중인 지역에 캐시 스토리지 계정이 만들어집니다.
2. 대상 스토리지 계정(주 지역의 원래 스토리지 계정)이 없는 경우 새로 만들어집니다. 할당된 스토리지 계정 이름은 보조 VM에서 사용하는 스토리지 계정의 이름이며 접미사 &quot;asr&quot;이 사용됩니다.
3. VM에서 관리 디스크를 사용하는 경우 복제본 관리 디스크는 보조 VM의 디스크에서 복제된 데이터를 저장하기 위해 주 지역에 생성됩니다.
4. 대상 가용성 집합이 없으면 다시 보호 작업의 일부로 필요한 경우 새로 만들어집니다. 다시 보호 설정을 사용자 지정한 경우 선택한 집합이 사용됩니다.

다시 보호 작업을 트리거하고 대상 VM이 있는 경우 다음이 발생합니다.

1. 대상 쪽 VM은 실행 중인 경우 꺼집니다.
2. VM이 관리 디스크를 사용하는 경우 원래 디스크의 사본에는 ‘-ASRReplica' 접미사가 붙습니다. 원래 디스크가 삭제됩니다. '-ASRReplica' 사본은 복제에 사용됩니다.
3. VM이 관리되지 않는 디스크를 사용할 경우 대상 VM의 데이터 디스크가 분리되어 복제에 사용됩니다. OS 디스크의 복사본이 만들어지고 VM에서 연결됩니다. 원래의 OS 디스크가 분리되며 복제에 사용됩니다.
4. 원본 디스크와 대상 디스크 간의 변경 내용만 동기화됩니다. 차등 백업은 디스크를 모두 비교하여 계산된 다음, 전달됩니다. 아래에서 예상 시간을 확인 하세요.
5. 동기화가 완료되면 델타 복제가 시작되고 복제 정책에 따라 복구 지점을 만듭니다.

다시 보호 작업을 트리거하고 대상 VM과 디스크가 없는 경우 다음이 발생합니다.
1. VM이 관리 디스크를 사용하는 경우 복제 디스크에는 '-ASRReplica' 접미사가 붙습니다. '-ASRReplica' 사본은 복제에 사용됩니다.
2. VM이 관리되지 않는 디스크를 사용하는 경우 복제 디스크는 대상 스토리지 계정에 만들어집니다.
3. 전체 디스크가 장애 조치된 지역에서 새 대상 지역에 복사됩니다.
4. 동기화가 완료되면 델타 복제가 시작되고 복제 정책에 따라 복구 지점을 만듭니다.

#### <a name="estimated-time--to-do-the-reprotection"></a>다시 보호 예상 시간 

대부분의 경우 Azure Site Recovery은 전체 데이터를 원본 영역에 복제 하지 않습니다. 다음은 복제 되는 데이터의 양을 결정 하는 조건입니다.

1.  원본 VM 데이터를 삭제, 손상 또는 액세스할 수 없는 경우에는 사용 하는 원본 지역에서 사용할 수 있는 데이터가 없으므로 다시 보호를 완료 하는 동안 리소스 그룹 변경/삭제와 같은 몇 가지 이유로 인해이 오류가 발생 합니다.
2.  원본 VM 데이터에 액세스할 수 있는 경우 두 디스크를 비교한 다음 전송 하 여 차등만 계산 합니다. 예상 시간을 얻으려면 아래 표를 확인 하세요. 

|\* * 예제 상황 * * | \* * 다시 보호 하는 데 걸린 시간 * * |
|--- | --- |
|원본 영역에 1TB standard 디스크가 있는 VM 1 개 있음<br/>-127 GB 데이터만 사용 되며 나머지 디스크는 비어 있습니다.<br/>-디스크 유형이 표준 이며 60 MiB/S 처리량<br/>-장애 조치 (failover) 후 데이터 변경 안 함| 대략적인 45 분 ~ 1.5 시간<br/> -다시 보호 Site Recovery 하는 동안 127 g b/45 Mb를 사용 하는 전체 데이터의 체크섬을 채웁니다. ~ 45 분<br/>-20-30 분의 자동 크기 조정을 수행 하는 Site Recovery에는 몇 가지 오버 헤드 시간이 필요 합니다.<br/>-송신 요금 없음 |
|원본 영역에 1TB standard 디스크가 있는 VM 1 개 있음<br/>-127 GB 데이터만 사용 되며 나머지 디스크는 비어 있습니다.<br/>-디스크 유형이 표준 이며 60 MiB/S 처리량<br/>-장애 조치 (failover) 후 45 GB 데이터 변경| 대략적인 시간 1 시간-2 시간<br/>-다시 보호 Site Recovery 하는 동안 127 g b/45 Mb를 사용 하는 전체 데이터의 체크섬을 채웁니다. ~ 45 분<br/>-45 45 GB/45 MBps ~ 17 분의 변경 사항을 적용 하기 위한 전송 시간<br/>-송신 요금은 체크섬에 대해 45 GB 데이터에만 해당 됩니다.|
 



## <a name="next-steps"></a>다음 단계

VM을 보호한 후 장애 조치(failover)를 시작할 수 있습니다. 장애 조치(failover)를 수행하면 보조 지역의 VM을 종료하고 잠시 가동 중지되었다가 주 지역에 VM을 만들어 부팅합니다. 시간을 적절하게 선택하고, 주 사이트에 전체 장애 조치(failover)를 시작하기 전에 테스트 장애 조치(failover)를 실행하는 것이 좋습니다. 장애 조치(failover)에 대해 [자세히 알아봅니다](site-recovery-failover.md) .
