---
title: 포함 파일
description: 포함 파일
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 12/06/2019
ms.openlocfilehash: 4d32980e825f12c76b5c8bf8df0673fa82065751
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460408"
---
1. [Azure portal](https://ms.portal.azure.com/)에서 **Azure Active Directory** > **앱 등록** > **새 등록**을 선택합니다.

   [![Azure Active Directory에서 새 애플리케이션 등록](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

    등록한 후에 앱이 여기 나열됩니다.

1. 애플리케이션 이름을 지정하고 **이 조직 디렉터리의 계정만**을 선택하여 API에 액세스할 수 있는 **지원되는 계정 유형**을 지정합니다. 인증된 후 사용자를 리디렉션할 유효한 URI를 선택한 다음, **등록**을 선택합니다.

   [![Azure Active Directory에서 애플리케이션 만들기](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. 중요 Azure Active Directory 앱 정보가 나열된 앱의 **개요** 블레이드에 표시됩니다. **소유한 애플리케이션**에서 앱을 선택한 다음, **개요**를 선택합니다.

   [![애플리케이션 ID 복사](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   클라이언트 애플리케이션에서 사용하도록 **애플리케이션(클라이언트) ID**를 복사합니다.

1. **인증** 블레이드에서는 중요한 인증 구성 설정을 지정합니다. 

    1. **리디렉션 URI**는 인증 요청에 의해 제공된 주소와 일치해야 합니다.

        * 로컬 개발 환경에서 호스팅되는 앱의 경우 **공용 클라이언트(모바일 및 데스크톱)** 를 선택합니다. **기본 클라이언트 형식**을 예로 설정해야 합니다.
        * Azure App Service에서 호스팅되는 단일 페이지 앱의 경우 **웹**을 선택합니다.

    1. **액세스 토큰** 또는 **ID 토큰**을 확인 하 여 암시적 허용 흐름을 사용 하도록 설정 합니다.

   [![리디렉션 Uri를 만들고 암시적 부여를 구성 합니다.](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   **저장**을 클릭합니다.

1. **인증서 및 비밀**을 선택한 다음, **새 클라이언트 비밀**을 선택하여 클라이언트에서 ID를 증명하는 데 사용할 수 있는 애플리케이션 암호를 만듭니다.

   [![새 클라이언트 비밀 만들기](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   그러면 클라이언트 비밀 암호가 표시됩니다. 원하는 텍스트 편집기에 키를 복사합니다.

   > [!NOTE]
   > 대신 인증서를 가져올 수 있습니다. 보안을 강화하기 위해 인증서를 사용하는 것이 좋습니다. 인증서를 사용하려면 **인증서 업로드**를 선택합니다.

1. Azure Active Directory 앱 Azure Time Series Insights에 연결합니다. **API 사용 권한** > **사용 권한 추가** > **내 조직에서 사용하는 API**를 선택합니다. 

    [![Azure Active Directory 앱을 사용하여 API에 연결](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   `Azure Time Series Insights`를 검색 창에 입력한 다음, `Azure Time Series Insights`를 선택합니다.

1. 다음으로 앱에 필요한 API 사용 권한 종류를 지정합니다. 기본적으로 **위임된 사용 권한**이 강조 표시됩니다. 사용 권한 유형을 선택한 다음, **사용 권한 추가**를 선택합니다.

    [![앱에 필요한 API 사용 권한 종류 지정](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)