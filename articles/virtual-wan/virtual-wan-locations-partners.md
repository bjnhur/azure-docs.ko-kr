---
title: Azure Virtual WAN 파트너 위치 | Microsoft Docs
description: 이 문서에는 Azure 가상 WAN 파트너 및 허브 위치 목록이 포함 되어 있습니다.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: b7a3d443b64943c097a51eeae4be6253a0466e18
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933707"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>가상 WAN 파트너 및 가상 허브 위치

이 문서에서는 가상 허브에 연결 하기 위해 가상 WAN 지원 지역 및 파트너에 대 한 정보를 제공 합니다.

Azure Virtual WAN은 Azure를 통해 최적화 및 자동화된 분기 간 연결을 제공하는 네트워킹 서비스입니다. Virtual WAN을 사용하여 Azure와 통신하도록 분기 디바이스를 연결 및 구성할 수 있습니다. 이러한 작업은 수동으로 수행 하거나 가상 WAN 파트너를 통해 공급자 장치를 사용 하 여 수행할 수 있습니다. 파트너 장치를 사용 하면 편리 하 고 간편 하 게 사용 하 고 연결을 간소화 하 고 구성할 수 있습니다.

온-프레미스 디바이스에서 가상 허브로의 연결은 자동화된 방식으로 설정됩니다. 가상 허브는 Microsoft에서 관리하는 가상 네트워크입니다. 허브는 온-프레미스 네트워크(vpnsite)에서 연결을 활성화하는 다양한 서비스 엔드포인트를 포함합니다. 지역당 허브는 하나만 있을 수 있습니다.

## <a name="automation"></a>연결 파트너의 자동화

Azure Virtual WAN에 연결하는 디바이스에는 연결 자동화가 기본으로 제공됩니다. 이 기능은 일반적으로 device-management UI(또는 이와 동등)에 설정되며, VPN 분기 디바이스와 Azure Virtual Hub VPN 엔드포인트(VPN 게이트웨이) 간의 연결 및 구성 관리를 설정합니다.

다음 상위 레벨 자동화가 디바이스 콘솔/관리 센터에 설정됩니다.

* 디바이스가 Azure Virtual WAN 리소스 그룹에 액세스하기 위한 적절한 권한
* Azure Virtual WAN에 분기 디바이스 업로드
* Azure 연결 정보 자동 다운로드
* 온-프레미스 분기 디바이스 구성 

일부 연결 파트너는 Azure Virtual Hub VNet과 VPN 게이트웨이 만들기를 포함하도록 자동화를 확장할 수 있습니다. 자동화에 대해 자세히 알려면 [자동화 구성 - WAN 파트너](virtual-wan-configure-automation-providers.md)를 참조하세요.

## <a name="partners"></a>파트너를 통한 연결

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

다음 파트너는 가까운 장래에는 Arista, 아루바 HPE, Cisco Systems, F5 Networks, Open Systems, Oracle Talari, SharpLink 및 VMWare Velocloud에 예정 되어 있습니다.

## <a name="locations"></a>위치

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>다음 단계

* 가상 WAN에 대한 자세한 내용은 [가상 WAN FAQ](virtual-wan-faq.md)를 참조하세요.

* Azure Virtual WAN에 대한 연결을 자동화하는 방법에 대한 자세한 내용은 [가상 WAN 파트너 - 자동화 방법](virtual-wan-configure-automation-providers.md)을 참조하세요.
