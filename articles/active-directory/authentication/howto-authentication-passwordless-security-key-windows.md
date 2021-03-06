---
title: Passwordless 보안 키 로그인 Windows-Azure Active Directory
description: FIDO2 보안 키 (미리 보기)를 사용 하 여 Azure AD에 암호 없는 보안 키 로그인 사용
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown, aakapo
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce2b420c2124c86610058ce2f31cd6d7bf620a97
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74848462"
---
# <a name="enable-passwordless-security-key-sign-in-to-windows-10-devices-preview"></a>Windows 10 장치에 대 한 암호 없는 보안 키 로그인 사용 (미리 보기)

이 문서에서는 Windows 10 장치에서 FIDO2 보안 키 기반 암호 없는 인증을 사용 하도록 설정 하는 방법을 집중적으로 설명 합니다. 이 문서의 끝부분에서는 FIDO2 보안 키를 사용 하 여 Azure AD 계정으로 웹 기반 응용 프로그램 및 Azure AD 조인 Windows 10 장치에 로그인 할 수 있습니다.

|     |
| --- |
| FIDO2 보안 키는 Azure Active Directory의 공개 미리 보기 기능입니다. 미리 보기에 대한 자세한 내용은 [Microsoft Azure 미리 보기에 대한 추가 사용 조건](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.|
|     |

## <a name="requirements"></a>요구 사항

- [Azure Multi-Factor Authentication](howto-mfa-getstarted.md)
- [결합 된 보안 정보 등록 미리 보기](concept-registration-mfa-sspr-combined.md)
- 호환 되는 [FIDO2 보안 키](concept-authentication-passwordless.md#fido2-security-keys)
- WebAuthN에는 Windows 10 버전 1809 이상이 필요 합니다.
- [AZURE AD 가입 장치](../devices/concept-azure-ad-join.md) 에는 Windows 10 버전 1809 이상이 필요 합니다.
- [Microsoft Intune](https://docs.microsoft.com/intune/fundamentals/what-is-intune) (옵션)
- 프로 비전 패키지 (옵션)

### <a name="unsupported-scenarios"></a>지원되지 않는 시나리오

- AD DS (Windows Server Active Directory Domain Services) 도메인에 가입 된 (온-프레미스 전용 장치) 배포는 **지원 되지 않습니다**.
- RDP, VDI 및 Citrix 시나리오는 보안 키를 사용 하 여 **지원 되지 않습니다** .
- S/MIME은 보안 키를 사용 하 여 **지원 되지 않습니다** .
- "실행"은 보안 키를 사용 하 여 **지원 되지 않습니다** .
- 보안 키를 사용 하 여 서버에 로그인 할 수 **없습니다**.
- 온라인 상태에서 보안 키를 사용 하 여 장치에 로그인 하지 않은 경우에는이를 사용 하 여 오프 라인으로 로그인 하거나 잠금 해제 하는 데 사용할 수 없습니다.
- 여러 Azure AD 계정을 포함 하는 보안 키를 사용 하 여 Windows 10 장치에 로그인 하거나 잠금을 해제 합니다. 이 시나리오는 보안 키에 추가 된 마지막 계정을 활용 합니다. 사용자는 WebAuthN를 사용 하 여 사용 하려는 계정을 선택할 수 있습니다.

## <a name="prepare-devices-for-preview"></a>미리 보기용으로 장치 준비

파일럿 할 Azure AD 조인 장치는 Windows 10 버전 1809 이상을 실행 해야 합니다. 최상의 환경은 Windows 10 버전 1903 이상에 있습니다.

## <a name="enable-security-keys-for-windows-sign-in"></a>Windows 로그인에 대 한 보안 키 사용

조직에서는 조직의 요구 사항에 따라 Windows 로그인에 보안 키를 사용할 수 있도록 다음 방법 중 하나 이상을 사용 하도록 선택할 수 있습니다.

- [Intune을 사용 하 여 사용](#enable-with-intune)
- [대상 Intune 배포](#targeted-intune-deployment)
- [프로 비전 패키지를 사용 하 여 사용](#enable-with-a-provisioning-package)

### <a name="enable-with-intune"></a>Intune을 사용 하 여 사용

1. [Azure portal](https://portal.azure.com)에 로그인합니다.
1. Windows **등록 > windows 등록** 을 >  > **장치 등록** 을 **Microsoft Intune** 하 여 **비즈니스용 windows Hello** > **속성**으로 이동 합니다.
1. **설정** 에서 **로그인에 대 한 보안 키 사용** 을 사용 **으로 설정**합니다.

로그인에 대 한 보안 키 구성은 비즈니스용 Windows Hello 구성에 종속 되지 않습니다.

### <a name="targeted-intune-deployment"></a>대상 Intune 배포

특정 장치 그룹을 대상으로 자격 증명 공급자를 사용 하도록 설정 하려면 Intune을 통해 다음 사용자 지정 설정을 사용 합니다.

1. [Azure portal](https://portal.azure.com)에 로그인합니다.
1. **Microsoft Intune** > **장치 구성** > 프로필로 이동 하 여 **프로필 만들기**를 > 합니다.
1. 다음 설정을 사용 하 여 새 프로필을 구성 합니다.
   1. 이름: Windows 로그인에 대 한 보안 키
   1. 설명: Windows 로그인 중에 FIDO 보안 키를 사용할 수 있습니다.
   1. 플랫폼: Windows 10 이상
   1. 프로필 유형: 사용자 지정
   1. 사용자 지정 OMA-URI 설정:
      1. 이름: Windows 로그인에 대 한 FIDO 보안 키를 설정 합니다.
      1. OMA-URI: ./Device/Vendor/MSFT/PassportForWork/SecurityKey/UseSecurityKeyForSignin
      1. 데이터 형식: Integer
      1. 값: 1
1. 이 정책은 특정 사용자, 장치 또는 그룹에 할당할 수 있습니다. 자세한 내용은 [Microsoft Intune에서 사용자 및 장치 프로필 할당](https://docs.microsoft.com/intune/device-profile-assign)문서에서 찾을 수 있습니다.

![Intune 사용자 지정 장치 구성 정책 만들기](./media/howto-authentication-passwordless-security-key/intune-custom-profile.png)

### <a name="enable-with-a-provisioning-package"></a>프로 비전 패키지를 사용 하 여 사용

Intune에서 관리 되지 않는 장치의 경우 기능을 사용 하도록 프로 비전 패키지를 설치할 수 있습니다. [Microsoft Store](https://www.microsoft.com/en-us/p/windows-configuration-designer/9nblggh4tx22)에서 Windows 구성 디자이너 앱을 설치할 수 있습니다.

1. Windows 구성 디자이너를 시작 합니다.
1. **파일** > **새 프로젝트**를 선택 합니다.
1. 프로젝트에 이름을 지정 하 고 프로젝트를 만든 경로를 기록해 둡니다.
1. **다음**을 선택합니다.
1. **선택한 프로젝트 워크플로** 로 **프로 비전 패키지** 를 선택 된 채로 두고 **다음**을 선택 합니다.
1. **모든 Windows 데스크톱 버전** 선택에서 **확인 및 구성할 설정을** 선택 하 고 **다음**을 선택 합니다.
1. **마침**을 선택합니다.
1. 새로 만든 프로젝트에서 **런타임 설정** > **WindowsHelloForBusiness** > **securitykeys** > **UseSecurityKeyForSignIn**로 이동 합니다.
1. **UseSecurityKeyForSignIn** 을 **Enabled**로 설정 합니다.
1. **내보내기** > **프로 비전 패키지** 를 선택 합니다.
1. **빌드** 창에서 **프로 비전 패키지 설명** 아래의 기본값을 그대로 두고 **다음**을 선택 합니다.
1. **빌드** 창에서 **프로 비전 패키지에 대 한 보안 세부 정보 선택** 의 기본값을 그대로 두고 **다음**을 선택 합니다.
1. **프로 비전 패키지를 저장할 위치 선택** 에서 **빌드** 창의 경로를 확인 하거나 변경 하 고 **다음**을 선택 합니다.
1. **프로 비전 패키지 빌드** 페이지에서 **빌드** 를 선택 합니다.
1. 만든 두 파일 (ppkg 및 cat)을 나중에 컴퓨터에 적용할 수 있는 위치에 저장 합니다.
1. [프로 비전 패키지 적용](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-apply-package)문서의 지침에 따라 만든 프로 비전 패키지를 적용 합니다.

> [!NOTE]
> Windows 10 버전 1809을 실행 하는 장치 에서도 공유 PC 모드 (EnableSharedPCMode)를 사용 하도록 설정 해야 합니다. 이 기능이를 사용 하는 방법에 대 한 정보는 [Windows 10을 사용 하 여 공유 또는 게스트 PC 설정](https://docs.microsoft.com/windows/configuration/set-up-shared-or-guest-pc)문서에서 찾을 수 있습니다.

## <a name="sign-in-to-windows-with-a-fido2-security-key"></a>FIDO2 보안 키로 Windows에 로그인

아래 예에서는 사용자 Bala Sandhu이 이전 문서의 단계를 사용 하 여 FIDO2 보안 키를 이미 프로 비전 했습니다. [암호 없는 보안 키 로그인을 사용 하도록 설정](howto-authentication-passwordless-security-key.md#user-registration-and-management-of-fido2-security-keys)합니다. Bala는 Windows 10 잠금 화면에서 보안 키 자격 증명 공급자를 선택 하 고 Windows에 로그인 하는 보안 키를 삽입할 수 있습니다.

![Windows 10 잠금 화면에서 보안 키 로그인](./media/howto-authentication-passwordless-security-key/fido2-windows-10-1903-sign-in-lock-screen.png)

### <a name="manage-security-key-biometric-pin-or-reset-security-key"></a>보안 키 생체 인식, PIN 또는 다시 설정 보안 키를 관리 합니다.

* Windows 10 버전 1903 이상
   * 사용자는 장치 > **계정** > **보안 키** 에서 **Windows 설정을** 열 수 있습니다.
   * 사용자가 PIN을 변경 하거나, 생체 인식을 업데이트 하거나, 보안 키를 다시 설정할 수 있습니다.

## <a name="troubleshooting-and-feedback"></a>문제 해결 및 피드백

이 기능을 미리 보는 동안 피드백을 공유 하거나 문제를 발생 시키려면 Windows 피드백 허브 앱을 통해 공유 하세요.

1. **피드백 허브** 를 시작 하 고 로그인 했는지 확인 합니다.
1. 다음 분류에 따라 사용자 의견을 제출 합니다.
   1. 범주: 보안 및 개인 정보
   1. 하위 범주: FIDO
1. 로그를 캡처하려면 **문제 다시 만들기** 옵션을 사용 합니다.

## <a name="frequently-asked-questions"></a>FAQ(질문과 대답)

### <a name="does-this-work-in-my-on-premises-environment"></a>온-프레미스 환경에서 작동 하나요?

이 기능은 순수한 온-프레미스 Active Directory Domain Services (AD DS) 환경에서 작동 하지 않습니다.

### <a name="my-organization-requires-two-factor-authentication-to-access-resources-what-can-i-do-to-support-this-requirement"></a>조직에서 리소스에 액세스 하려면 2 단계 인증을 요구 하 고,이 요구 사항을 지원 하기 위해 수행할 수 있는 작업은 무엇 인가요?

보안 키는 다양 한 폼 팩터를 제공 합니다. 원하는 장치 제조업체에 문의 하 여 PIN 또는 생체 인식으로 두 번째 요소로 장치를 사용 하도록 설정 하는 방법에 대해 논의 하세요.

### <a name="can-admins-set-up-security-keys"></a>관리자가 보안 키를 설정할 수 있나요?

이 기능의 GA (일반 공급)에 대해이 기능을 사용 하 고 있습니다.

### <a name="where-can-i-go-to-find-compliant-security-keys"></a>규격 보안 키를 찾을 수 있는 위치

[FIDO2 보안 키](concept-authentication-passwordless.md#fido2-security-keys)

### <a name="what-do-i-do-if-i-lose-my-security-key"></a>보안 키를 분실 한 경우 어떻게 해야 하나요?

보안 정보 페이지로 이동 하 고 보안 키를 제거 하 여 Azure Portal에서 키를 제거할 수 있습니다.

## <a name="next-steps"></a>다음 단계

[장치 등록에 대 한 자세한 정보](../devices/overview.md)

[Azure Multi-Factor Authentication에 대 한 자세한 정보](../authentication/howto-mfa-getstarted.md)
