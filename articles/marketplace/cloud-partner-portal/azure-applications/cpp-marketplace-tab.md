---
title: Azure 응용 프로그램 제공 Marketplace 탭
description: Marketplace 탭을 사용하여 Azure 애플리케이션 제품의 마케팅 자산을 식별합니다.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pabutler
ms.openlocfilehash: 967b66a67d51b3a79bcf930ce977af48acc3dd63
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827572"
---
# <a name="azure-application-marketplace-tab"></a>Azure 애플리케이션 Marketplace 탭

Marketplace 탭을 사용하여 Azure 애플리케이션을 설명하고 마케팅 자산을 제공합니다. 이 탭에는 개요, 마케팅 아티팩트, 잠재 고객 관리 및 법률 양식이 포함 됩니다.

## <a name="overview-form"></a>개요 양식

개요 양식에는 다음 화면 캡처에 표시된 필수 및 선택적 필드가 있습니다. 필수 필드는 별표(*)로 표시됩니다.

![개요 양식](./media/azureapp-marketplace-overview.png)

다음 표에서는 제품용 상점을 만드는 데 사용할 설정을 설명합니다.   별표가 추가 된 필드가 필요 합니다.

|      필드         |    설명    |
|  ---------------   |  ---------------  |
| **제목\***        | 제품의 제목입니다. 마켓플레이스에서 눈에 띄게 표시됩니다. 최대 길이는 50자입니다. |
| **요약\***      | 제품의 간단한 요약입니다. 최대 길이는 100자입니다.           |
| **긴 요약\*** | 제품의 더 긴 요약입니다(요약과 동일할 수 있음). 최대 길이는 256자입니다.           |
| **설명\***  | 제품에 대한 설명입니다. 최대 길이는 3,000자입니다. &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; 및 header 태그를 포함하는 간단한 HTML이 허용됩니다.  |
| **마케팅 식별자\*** | 이 제안과 연결할 고유 URL이며, 일반적으로 조직 및 솔루션 이름을 포함하고 최대 길이는 50자입니다. 서비스에 대한 짧고 친숙한 마케팅 식별자를 선택합니다. 이 제품의 Marketplace URL에서 사용됩니다. 예를 들어 게시자 ID가 "contoso"이 고 마케팅 id가 "Sampleapp.exe" 인 경우 Azure Marketplace의 제품에 대 한 URL이 https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp 됩니다.  
| **구독 Id 미리 보기\*** | 1~100개의 미리 보기 구독 식별자를 추가합니다. 이러한 항목에 나열 된 구독은 게시 된 후 미리 보기에서 사용할 수 있는 동안 해당 제품에 대 한 액세스 권한을 보유 하 게 됩니다.          |
| **유용한 링크**    | 필요에 따라 제품의 사용자에 대 한 다양 한 리소스 (예: 지원, 설명서, 포럼 등)에 대 한 링크를 제공할 수 있습니다.  설명서에 대 한 링크를 하나 이상 추가 하는 것이 좋습니다.            |
| **제안 된 범주 (최대 5 개)\*** | 범주를 5 개까지 선택 합니다. 선택한 범주는 고객의 제품을 Azure Marketplace 및 Azure Portal에서 사용할 수 있는 제품 범주에 매핑하는 데 사용됩니다. 이러한 범주는 찾아보기 페이지 및 제품 세부 정보 페이지에 표시됩니다. |
|  |  |


## <a name="marketing-artifacts"></a>마케팅 아티팩트

마케팅 아티팩트 양식에는 다음 화면 캡처에 표시된 필수 및 선택적 필드가 있습니다. 필수 필드는 별표(*)로 표시됩니다.

![마케팅 아티팩트 양식](./media/azureapp-marketplace-artifacts.png)

다음 표는 마케팅 아티팩트에 대해 설명합니다.

|      필드         |    설명    |
|  ---------------   |  ---------------  |
| **작음\***        | 작은 로고: PNG 형식의 40x40 픽셀     |
| **중간\***       | 중간 로고: 90x90 픽셀 (PNG 형식)    |
| **큼\***        | 크게 로고: PNG 형식의 115x115 픽셀   |
| **넓은\***         | 와이드 로고: PNG 형식의 255x115 픽셀    |
| **대표**           | 선택적 주인공 로고: (PNG 형식의 815x290 픽셀) **참고:** 주인공 아이콘은 업로드 한 후에는 삭제할 수 없습니다. |
| **스크린샷(최대 5개)** |        스크린샷은 제품 세부 정보 페이지에 표시됩니다. 앱의 의미 및 작동 방식을 시각적으로 알려주는 좋은 방법입니다. 예를 들어 아키텍처 다이어그램이나 사용 사례 그림을 표시할 수 있습니다. 스크린샷은 선택 사항이며 SKU당 5개로 제한됩니다. 스크린샷을 추가하려면<ul><li>**+ 스크린샷 추가**를 선택하여 스크린샷 창을 엽니다.</li><li>**이름** - 이름/제목(최대 100자)을 입력합니다.</li><li>**업로드** - 이미지를 업로드합니다. PNG 형식이어야 하며 크기는 533x324 픽셀입니다.</li></ul>           |
| **비디오 추가**      | 선택 사항으로, 비디오는 제품 세부 정보 페이지에 표시 됩니다. 애플리케이션의 의미 및 작동 방식을 시각적으로 알려주는 좋은 방법입니다. 비디오를 추가하려면 <ul><li>**+ 비디오 추가**를 선택하여 비디오 창을 엽니다.</li><li>**이름** - 이름/제목(최대 100자)을 입력합니다.</li><li>**링크** -비디오를 호스트 하는 사이트의 URL (YouTube 또는 Vimeo)을 입력 합니다.</li><li>**미리 보기** -미리 보기를 업로드 합니다. PNG 형식이어야 하며 크기는 533x324 픽셀입니다.</li></ul>          |
|  |  |


### <a name="artifact-examples-in-azure-marketplace"></a>Azure Marketplace의 아티팩트 예제

다음 화면 캡처에는 Marketplace 검색 결과의 예가 나와 있습니다.

![Marketplace 제품 검색 결과](./media/azureapp-marketplace-example-browse.png)

다음 이미지는 고객이 검색 결과에서 제품의 타일을 클릭 한 후 Marketplace에 제품을 표시 하는 방법을 보여 줍니다.

![Marketplace 제품 검색 결과 세부 정보](./media/azureapp-marketplace-example-details.png)


### <a name="artifact-examples-in-azure-portal"></a>Azure Portal의 아티팩트 예제

다음 화면 캡처는 Azure Portal에 제품이 표시되는 방법을 보여 줍니다. 이 예제의 애플리케이션 제품은 **Marketplace>모든 항목>개발 + 테스트>Jenkins**로 이동하면 표시됩니다. Jenkins 제품은 로고, 제목 및 게시자 표시 이름을 보여 줍니다.

![Azure Portal에서 제품 찾아보기](./media/azureapp-portalbrowse-artifacts-jenkins.png)

다음 화면 캡처는 Jenkins를 선택할 때의 애플리케이션에 대한 자세한 정보를 표시합니다.

![Azure Portal의 제품 세부 정보](./media/azureapp-portal-artifacts-jenkins-details.png)


### <a name="logo-guidelines"></a>로고 지침

Cloud 파트너 포털에 업로드되는 모든 로고는 다음 지침을 따라야 합니다.

- Azure 디자인은 단순한 색 팔레트를 사용합니다. 로고의 기본 색상과 보조 색상 수는 적게 유지합니다.
- Azure Portal의 테마 색은 흰색과 검은색입니다. 로고의 배경색으로는 이러한 색을 사용하지 마세요. Azure Portal에서 로고가 돋보이도록 하는 색을 사용합니다. 간단한 기본 색을 사용하는 것이 좋습니다. 투명한 배경을 사용하는 경우 로고/텍스트가 흰색, 검은색 또는 파란색이 아닌지 확인합니다.
- 로고의 배경에 그라데이션 효과를 사용하지 않습니다.
- 로고에 회사 또는 브랜드 이름을 포함한 텍스트를 놓지 않습니다. 로고의 모양과 느낌은 "평면적"이어야 하며 그라데이션은 사용하지 않습니다.
- 로고를 늘리지 않습니다.


#### <a name="hero-logo"></a>대표 로고

대표 로고는 선택 사항입니다.

>[!IMPORTANT]
>업로드 한 후에는 주인공 로고를 삭제할 수 없습니다.

대표 로고에 다음 지침을 사용합니다.

- 검은색, 흰색 및 투명한 배경이 허용되지 않습니다.
- 밝은 색을 로고의 배경으로 사용하지 않습니다. 게시자 표시 이름, 계획 제목 및 긴 제품 요약은 흰색 글꼴 색으로 표시되며 배경에서 눈에 잘 띄어야 합니다.
- 로고를 디자인할 때 대부분의 텍스트를 사용하지 마십시오. 제품이 나열되면 로고 내에 게시자 이름, 계획 제목, 긴 제품 요약 및 만들기 단추가 프로그래밍 방식으로 포함됩니다.
- 대표 로고의 오른쪽에는 사용되지 않는 사각형 공간을 포함합니다. 이 빈 공간은 415x100 픽셀이며, 왼쪽에서 370 픽셀만큼 오프셋됩니다.


## <a name="lead-management"></a>잠재 고객 관리

잠재 고객 관리 양식에는 잠재 고객 관리를 구성하기 위한 선택적 필드가 있습니다. 잠재 고객 관리를 구성하려면 드롭다운 목록에서 잠재 고객 대상을 선택합니다. 다음 화면 캡처는 사용할 수 있는 대상을 보여 줍니다.

![잠재 고객 관리 대상 선택](./media/azureapp-marketplace-leadmgmt.png)

>[!TIP]
>정보 아이콘을 선택 하 여이 메시지를 표시 합니다. "잠재 고객이 저장 될 시스템을 선택 합니다. [여기](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) 에서 CRM 시스템에 연결 하는 방법을 알아봅니다. "

자세한 내용은 [잠재 고객 구성](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)을 참조하세요.


## <a name="legal"></a>법적 정보

법적 정보 양식을 사용하여 모든 제품에 필요한 법적 정보 설명서를 제공합니다.

다음 정보를 지정합니다.

- **개인 정보 취급 방침 URL\*** -앱의 개인 정보 취급 방침에 대 한 링크를 입력 합니다.
- **사용 약관\*** 앱에 대 한 사용 약관을 입력 합니다. 고객은 이러한 사용 약관에 동의해야 앱을 사용할 수 있습니다.

![법적 정보 양식](./media/azureapp-marketplace-legal.png)


## <a name="next-steps"></a>다음 단계

[지원 탭](./cpp-support-tab.md)
