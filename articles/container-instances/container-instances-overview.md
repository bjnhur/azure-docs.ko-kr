---
title: Azure의 서버리스 컨테이너
description: Azure Container Instances 서비스는 가상 머신을 관리하지 않고 또 더 높은 수준의 오케스트레이터를 채택하지 않고도 Azure에서 격리된 컨테이너를 실행하는 가장 빠르고 간단한 방법을 제공합니다.
ms.topic: overview
ms.date: 04/25/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: c871c09e29b64c4f0dcd107361154efdce306481
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75888036"
---
# <a name="what-is-azure-container-instances"></a>Azure Container Instances란?

컨테이너는 클라우드 애플리케이션을 패키지, 배포 및 관리하기 위한 기본 방법으로 도입되고 있습니다. Azure Container Instances는 어떠한 가상 머신도 관리하지 않고 또 더 높은 수준의 서비스를 채택하지 않고도 Azure에서 컨테이너를 실행하는 가장 빠르고 간단한 방법을 제공합니다.

Azure Container Instances는 간단한 애플리케이션, 작업 자동화 및 빌드 작업 등 격리된 컨테이너에서 작동할 수 있는 모든 시나리오에 적합한 솔루션입니다. 여러 컨테이너 간 서비스 검색, 자동 크기 조정 및 조정된 애플리케이션 업그레이드를 포함하여 전체 컨테이너 오케스트레이션이 필요한 시나리오에는 [AKS(Azure Kubernetes Service)](../aks/index.yml)를 사용하는 것이 좋습니다.

## <a name="fast-startup-times"></a>빠른 시작 시간

컨테이너는 VM(가상 머신)에 비해 상당한 시작 이점을 제공합니다. Azure Container Instances는 VM을 프로비전 및 관리할 필요 없이 Azure에서 몇 초 안에 컨테이너를 시작할 수 있습니다.

## <a name="container-access"></a>컨테이너 액세스

Azure Container Instances는 IP 주소 및 FQDN(정규화된 도메인 이름)을 사용하여 컨테이너 그룹을 인터넷에 직접 노출할 수 있습니다. 컨테이너 인스턴스를 만들 때 사용자 정의 DNS 이름 레이블을 지정하여 *customlabel*.*azureregion*.azurecontainer.io에서 애플리케이션에 연결할 수 있습니다.

또한 Azure Container Instances는 애플리케이션 개발 및 문제 해결에 도움이 되는 대화형 셸을 제공하여 실행 중인 컨테이너에서 명령 실행을 지원합니다. 액세스는 HTTPS를 통해 이루어지며, TLS를 사용하여 클라이언트 연결을 보호합니다.

> [!IMPORTANT]
> 2020년 1월 13일부터 Azure Container Instances는 TLS 1.2를 사용하기 위해 서버 및 애플리케이션의 모든 보안 연결이 필요합니다. TLS 1.0 및 1.1에 대한 지원이 중단됩니다.

## <a name="hypervisor-level-security"></a>하이퍼바이저 수준 보안

지금까지는 컨테이너가 애플리케이션 종속성 격리 및 리소스 관리를 제공했지만 적대적인 다중 테넌트 사용을 위해서 충분히 보강되지 않았습니다. Azure Container Instances는 애플리케이션이 VM에서 격리되는 것처럼 컨테이너에서도 격리되도록 보장합니다.


## <a name="custom-sizes"></a>사용자 지정 크기

일반적으로 컨테이너는 단일 애플리케이션만 실행하도록 최적화되었지만 이러한 애플리케이션의 정확한 요구 사항은 크게 다를 수 있습니다. Azure Container Instances는 CPU 코어 및 메모리의 정확한 사양을 허용하여 최적의 활용도를 제공합니다. 필요한 만큼 비용을 초 단위로 지불하므로 실제로 필요한 양에 따라 지출을 미세하게 조정할 수 있습니다.

Machine Learning과 같은 컴퓨팅 집약적인 작업의 경우 Azure Container Instances는 NVIDIA Tesla [GPU 리소스](container-instances-gpu.md)(미리 보기)를 사용하도록 Linux 컨테이너를 예약할 수 있습니다.

## <a name="persistent-storage"></a>영구 스토리지

Azure Container Instances를 통해 상태를 검색하고 유지하기 위해 Azure Storage가 지원하는 [Azure Files 공유 탑재](container-instances-mounting-azure-files-volume.md)를 직접 제공합니다.

## <a name="linux-and-windows-containers"></a>Linux 및 Windows 컨테이너

Azure Container Instances는 동일한 API로 Windows 및 Linux 컨테이너를 모두 예약할 수 있습니다. [컨테이너 그룹](container-instances-container-groups.md)을 만들 때는 OS 종류만 지정합니다.

일부 기능은 현재 Linux 컨테이너에 제한됩니다.

* 컨테이너 그룹당 다중 컨테이너 수
* 볼륨 탑재([Azure Files](container-instances-volume-azure-files.md), [emptyDir](container-instances-volume-emptydir.md), [GitRepo](container-instances-volume-gitrepo.md), [secret](container-instances-volume-secret.md))
* Azure Monitor로 [리소스 사용량 메트릭](container-instances-monitor.md)
* [가상 네트워크 배포](container-instances-vnet.md)
* [GPU 리소스](container-instances-gpu.md)(미리 보기)

Windows 컨테이너 배포의 경우 일반적인 [Windows 기본 이미지](container-instances-faq.md#what-windows-base-os-images-are-supported)를 기반으로 하는 이미지를 사용합니다.

> [!NOTE]
> Azure Container Instances에서 Windows Server 2019 기반 이미지 사용은 미리 보기에 있습니다.

## <a name="co-scheduled-groups"></a>공동 예약된 그룹

Azure Container Instances는 호스트 컴퓨터, 로컬 네트워크, 스토리지 및 수명 주기를 공유하는 [다중 컨테이너 그룹](container-instances-container-groups.md)의 예약을 지원합니다. 이렇게 하면 주 애플리케이션 컨테이너를 로깅 사이드카 같은 다른 지원 역할 컨테이너와 결합할 수 있습니다.

## <a name="virtual-network-deployment"></a>가상 네트워크 배포

현재 Azure 지역의 하위 집합에서 프로덕션 워크로드에 사용할 수 있는 Azure Containers Instances의 이 기능을 통해 [Azure 가상 네트워크에 컨테이너 인스턴스를 배포](container-instances-vnet.md)하도록 설정할 수 있습니다. 컨테이너 인스턴스를 가상 네트워크 내의 서브넷에 배포하면 [VPN 게이트웨이](../vpn-gateway/vpn-gateway-about-vpngateways.md) 또는 [ExpressRoute](../expressroute/expressroute-introduction.md)를 통해 온-프레미스에 있는 컨테이너 인스턴스를 포함하여 가상 네트워크의 다른 리소스와 안전하게 통신할 수 있습니다.

## <a name="next-steps"></a>다음 단계

빠른 시작 가이드를 사용하여 단일 명령으로 Azure에 컨테이너를 배포해 보세요.

> [!div class="nextstepaction"]
> [Azure Container Instances 빠른 시작](container-instances-quickstart.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
