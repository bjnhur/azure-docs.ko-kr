---
title: Microsoft Azure Site Recovery 공급자 업그레이드 문제 해결
description: Microsoft Azure Site Recovery 공급자를 업그레이드할 때 발생 하는 일반적인 문제를 해결 합니다.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/10/2019
ms.author: raynew
ms.openlocfilehash: b59f933fedd5f1d3ed3f7972b1a1fe653df31be2
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75893906"
---
# <a name="troubleshoot-microsoft-azure-site-recovery-provider-upgrade-failures"></a>Microsoft Azure Site Recovery 공급 기업 업그레이드 오류 문제 해결

이 문서는 Microsoft Azure Site Recovery Provider 업그레이드 중에 실패 원인이 될 수 있는 문제를 해결하는 데 도움이 됩니다.

## <a name="the-upgrade-fails-reporting-that-the-latest-site-recovery-provider-is-already-installed"></a>업그레이드가 실패하고, 최신 Site Recovery 공급자가 이미 설치되어 있다고 보고합니다.

Microsoft Azure Site Recovery Provider(DRA)를 업그레이드하는 경우 통합 설치 업그레이드가 실패하고 오류 메시지가 표시됩니다.

상위 버전의 소프트웨어가 이미 설치되어 있으므로 업그레이드가 지원되지 않습니다.

업그레이드하려면 다음 단계를 수행합니다.

1. Microsoft Azure Site Recovery 통합 설치를 다운로드합니다.
   1. [Azure Site Recovery의 서비스 업데이트](service-updates-how-to.md#links-to-currently-supported-update-rollups) 문서, “현재 지원되는 업데이트 롤업에 대한 링크” 섹션에서 업그레이드할 공급자를 선택합니다.
   2. 롤업 페이지에서 **업데이트 정보** 섹션을 찾아 Microsoft Azure Site Recovery 통합 설치용 업데이트 롤업을 다운로드합니다.

2. 명령 프롬프트를 열고 통합 설치 파일을 다운로드한 폴더로 이동합니다. MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;folder path for the extracted files&gt; 명령을 사용하여 다운로드에서 설치 파일을 추출합니다.
    
    예제 명령:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

3. 명령 프롬프트에서 파일을 추출한 폴더로 이동하여 다음 설치 명령을 실행합니다.
   
    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART  UCX_SERVER_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /UPGRADE

1. 통합 설치를 다운로드한 폴더로 돌아가서 MicrosoftAzureSiteRecoveryUnifiedSetup.exe를 실행하여 업그레이드를 완료합니다. 

## <a name="upgrade-failure-due-to-the-3rd-party-folder-being-renamed"></a>이름을 바꾼 타사 폴더로 인해 업그레이드 하지 못했습니다.

업그레이드를 성공적으로 수행 하려면 타사 폴더의 이름을 바꾸지 않아야 합니다.

이 문제를 해결하려면 다음을 수행합니다.

1. 레지스트리 편집기(regedit.exe)를 시작하고 HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\InMage Systems\Installed Products\10 분기를 엽니다.
1. `Build_Version` 키 값을 검사합니다. 최신 버전으로 설정된 경우 버전 번호를 낮춥니다. 예를 들어 최신 버전이 9.22.\*이고 `Build_Version` 키가 해당 값으로 설정된 경우 9.21.\*로 낮춥니다.
1. 최신 Microsoft Azure Site Recovery 통합 설치를 다운로드합니다.
   1. [Azure Site Recovery의 서비스 업데이트](service-updates-how-to.md#links-to-currently-supported-update-rollups) 문서, “현재 지원되는 업데이트 롤업에 대한 링크” 섹션에서 업그레이드할 공급자를 선택합니다.
   2. 롤업 페이지에서 **업데이트 정보** 섹션을 찾아 Microsoft Azure Site Recovery 통합 설치용 업데이트 롤업을 다운로드합니다.
1. 명령 프롬프트를 열고 통합 설치 파일을 다운로드한 폴더로 이동한 다음, MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:&lt;folder path for the extracted files&gt; 명령을 사용하여 다운로드에서 설치 파일을 추출합니다.

    예제 명령:

    MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted

1. 명령 프롬프트에서 파일을 추출한 폴더로 이동하여 다음 설치 명령을 실행합니다.
   
    CX_THIRDPARTY_SETUP.EXE /VERYSILENT /SUPPRESSMSGBOXES /NORESTART

1. 작업 관리자를 사용하여 설치 진행 상황을 모니터링합니다. CX_THIRDPARTY_SETUP.EXE에 대한 프로세스가 작업 관리자에 더 이상 표시되지 않는 경우 다음 단계로 진행합니다.
1. C:\thirdparty가 있고 폴더에 RRD 라이브러리가 들어 있는지 확인합니다.
1. 통합 설치를 다운로드한 폴더로 돌아가서 MicrosoftAzureSiteRecoveryUnifiedSetup.exe를 실행하여 업그레이드를 완료합니다. 