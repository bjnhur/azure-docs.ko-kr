---
title: Azure Security Center의 보안 연락처 세부 정보 제공 | Microsoft Docs
description: 이 문서에서는 Azure Security Center에서 보안 연락처 세부 정보를 제공하는 방법을 보여 줍니다.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2019
ms.author: memildin
ms.openlocfilehash: 1a66ea200082f60a3a763c6a4e2bdea62ec473d8
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75920994"
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Azure Security Center의 보안 연락처 세부 정보 제공
Azure Security Center는 아직 없는 경우 Azure 구독에 대한 보안 연락처 세부 정보를 제공하는 것을 권장합니다. 이 정보는 MSRC(Microsoft 보안 대응 센터)에서 불법적인 또는 권한 없는 당사자가 고객 데이터에 액세스한 것을 발견하는 경우 사용자에게 연락하기 위해 Microsoft에서 사용됩니다. MSRC는 Azure 네트워크 및 인프라의 선택 보안 모니터링을 수행하고 타사에서 위협 인텔리전스 및 남용 불만 사항을 받습니다.

최초로 일별 경고가 발생할 때와 높은 심각도 경고가 발생하는 경우에만 전자 메일 알림이 전송됩니다. 전자 메일 기본 설정은 구독 정책에 대해서만 구성할 수 있습니다. 구독 내의 리소스 그룹은 이러한 설정을 상속합니다. 경고는 Azure Security Center 표준 계층 에서만 사용할 수 있습니다.

다음과 같이 경고 이메일 알림이 전송됩니다.
- 심각도가 높은 경고에 대해서만
- 일별 경고 유형당 1명의 이메일 받는 사람에게  
- 하루에 1명의 받는 사람에게 3개 이하의 이메일 메시지가 전송됩니다.
- 각 이메일 메시지에는 경고 집계가 아닌 단일 경고가 포함됩니다.
 
예를 들어, RDP 공격을 경고하는 이메일 메시지가 이미 전송된 경우 다른 경고가 트리거되더라도 같은 날에 RDP 공격에 대한 다른 이메일 메시지가 전송되지 않습니다. 

> [!NOTE]
> 이 문서에서는 배포 예제를 사용하여 서비스를 소개합니다.  단계별 가이드는 아닙니다.

## 경고에 대 한 전자 메일 알림 설정<a name="email"></a>

1. 포털에서 **가격 책정 & 설정**을 선택 합니다.
1. 구독을 클릭합니다.
1. **이메일 알림**을 클릭합니다.

> [!NOTE]
> 권장 사항을 구현 하는 경우 **권장 사항**에서 **보안 연락처 세부 정보 제공**을 선택 하 고 연락처 정보를 제공할 Azure 구독을 선택 합니다. **이메일 알림**을 엽니다.

   ![보안 연락처 세부 정보 제공][2]

   * 쉼표로 구분하여 보안 연락처 전자 메일 주소 또는 주소를 입력합니다. 입력할 수 있는 전자 메일 주소 수에 제한이 없습니다.
   * 하나의 보안 연락처 국제 전화 번호를 입력합니다.
   * 심각도 높은 경고에 대해 전자 메일을 받으려면 **경고에 대한 전자 메일 받기**옵션을 설정합니다.
   * 구독 소유자에 게 전자 메일 알림을 보내는 옵션 (클래식 서비스 관리자와 공동 관리자 및 구독 범위에서 RBAC 소유자 역할)을 사용할 수 있습니다.
   * **저장**을 선택하여 보안 연락처 정보를 구독에 적용합니다.

## <a name="see-also"></a>참고 항목
보안 센터에 대한 자세한 내용은 다음을 참조하세요.

* [Azure Security Center에서 보안 정책 설정](tutorial-security-policy.md) -- Azure 구독 및 리소스 그룹에 대해 보안 정책을 구성하는 방법을 알아봅니다.
* [Azure Security Center에서 보안 권장 사항 관리](security-center-recommendations.md) -- 권장 사항이 Azure 리소스 보호에 어떤 도움이 되는지를 알아봅니다.
* [Azure Security Center에서 보안 상태 모니터링](security-center-monitoring.md) –- Azure 리소스의 상태를 모니터링하는 방법을 알아봅니다.
* [Azure Security Center에서 보안 경고 관리 및 대응](security-center-managing-and-responding-alerts.md) - 보안 경고를 관리하고 대응하는 방법을 알아봅니다.
* [Azure Security Center를 사용하여 파트너 솔루션 모니터링](security-center-partner-solutions.md) -- 파트너 솔루션의 상태를 모니터링하는 방법을 알아봅니다.
* [Azure Security Center FAQ](security-center-faq.md) - 서비스 사용에 관한 질문과 대답을 찾습니다.
* [Azure 보안 블로그](https://blogs.msdn.com/b/azuresecurity/) -- 최신 Azure 보안 뉴스 및 정보를 가져옵니다.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
