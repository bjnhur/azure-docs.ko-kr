---
title: Azure 애플리케이션 관리되는 애플리케이션 제품 게시 가이드
description: 이 아티클에서는 관리되는 애플리케이션을 Marketplace에 게시하기 위한 요구 사항을 설명합니다.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: MaggiePucciEvans
manager: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 06/14/2018
ms.author: evansma
ms.openlocfilehash: 764212ac148b336b07d29c29a72314c5d889d47c
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934644"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure 애플리케이션: 관리되는 애플리케이션 제품 게시 가이드

관리되는 애플리케이션은 Marketplace에서 솔루션을 게시하는 주요 방법 중 하나입니다. 이 가이드를 사용하여 이 제품에 대한 요구 사항을 이해합니다. 

Marketplace를 통해 배포되고 청구되는 트랜잭션 제품은 다음과 같습니다. 사용자에게 표시되는 작업에 대한 호출은 "지금 가져오기"입니다.

다음 조건이 필요한 경우 Azure 앱: 관리되는 앱 제품 형식을 사용합니다.
- VM 또는 전체 IaaS 기반 솔루션을 사용하여 고객에게 구독 기반 솔루션을 배포합니다.
- 사용자 또는 고객이 솔루션을 파트너가 관리하도록 요구합니다.

>[!NOTE]
>예를 들어, 파트너는 SI 또는 MSP(관리 서비스 제공자)일 수 있습니다.  

## <a name="managed-application-offer"></a>관리되는 애플리케이션 제품

|요구 사항 |세부 정보  |
|---------|---------|
|고객의 Azure 구독에 배포 | 관리 되는 앱은 고객의 구독에 배포 되어야 하 고 타사에서 관리할 수 있습니다. | 
|청구 및 계량    |  리소스는 고객의 Azure 구독에서 프로 비전 됩니다. 종 량 제 (PAYGO) 가상 머신은 고객의 Azure 구독을 통해 청구 되는 Microsoft를 통해 고객에 게 거래 됩니다 (PAYGO). <br> 사용자 라이선스 가져오기의 경우 Microsoft는 고객 구독에서 발생 하는 인프라 비용을 청구 하지만 고객에 게 직접 소프트웨어 라이선스 요금을 지불 합니다.        |
|Azure 호환 VHD(가상 하드 디스크)    |   VM은 Windows 또는 Linux에서 빌드해야 합니다.<ul> <ul> <li>Linux VHD를 만드는 방법에 대한 자세한 내용은 [Azure 보증 Linux 배포판](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)을 참조하세요.</li> <li>Windows VHD를 만드는 방법에 대한 자세한 내용은 [Azure 호환 VHD 만들기](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md)를 참조하세요.</li> </ul> |

>[!NOTE]
> 관리되는 앱은 마켓플레이스를 통해 배포 가능해야 합니다. 고객 커뮤니케이션이 중요한 경우, 잠재 고객 공유를 사용하도록 설정한 후 관심 있는 고객에게 연결해야 합니다.  

>[!Note]
>이제 CSP (클라우드 솔루션 공급자) 파트너 채널 옵트인을 사용할 수 있습니다.  Microsoft CSP 파트너 채널을 통한 제품 마케팅에 대 한 자세한 내용은 [클라우드 솔루션 공급자](./cloud-solution-providers.md) 를 참조 하세요.

## <a name="next-steps"></a>다음 단계
아직 수행하지 않았다면 

- Marketplace에 [등록](https://azuremarketplace.microsoft.com/sell).

새 제품을 등록하고 만들거나 기존에서 작업하는 경우

- [Cloud 파트너 포털에 로그인](https://cloudpartner.azure.com) 하 여 제품을 만들거나 완성 합니다.
