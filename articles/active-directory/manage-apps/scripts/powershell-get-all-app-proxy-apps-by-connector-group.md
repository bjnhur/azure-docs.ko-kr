---
title: PowerShell 샘플-모든 응용 프로그램 프록시 커넥터 그룹 나열
description: 할당 된 응용 프로그램을 사용 하는 모든 Azure Active Directory (Azure AD) 응용 프로그램 프록시 커넥터 그룹을 나열 하는 PowerShell 예제입니다.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9dcf2eb6a1904e4a7223a19e598f0c06ddc36f71
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75482167"
---
# <a name="get-all-application-proxy-apps-and-list-by-connector-group"></a>커넥터 그룹별로 모든 응용 프로그램 프록시 앱 및 목록 가져오기

이 PowerShell 스크립트 예제는 할당 된 응용 프로그램을 사용 하 여 모든 Azure Active Directory (Azure AD) 응용 프로그램 프록시 커넥터 그룹에 대 한 정보를 나열 합니다.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

이 샘플에는 [AzureAD V2 powershell For graph 모듈](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) 또는 [AzureAD V2 powershell for graph module preview 버전](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview)이 필요 합니다.

## <a name="sample-script"></a>샘플 스크립트

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-by-connectorgroup.ps1 "Get all Application Proxy Connector groups with the assigned applications")]

## <a name="script-explanation"></a>스크립트 설명

| 명령 | 메모 |
|---|---|
|[Get-azureadserviceprincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | 서비스 사용자를 가져옵니다. |
|[Azureadapplication.applicationid](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Azure AD 응용 프로그램을 가져옵니다. |
|[AzureADApplicationProxyApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | Azure AD에서 응용 프로그램 프록시에 대해 구성 된 응용 프로그램을 검색 합니다. |
| [AzureADApplicationProxyConnectorGroup](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyconnectorgroup?view=azureadps-2.0) | 모든 커넥터 그룹 목록을 검색 하거나 지정 된 경우 지정 된 커넥터 그룹의 세부 정보를 검색 합니다. |


## <a name="next-steps"></a>다음 단계

Azure AD PowerShell 모듈에 대 한 자세한 내용은 [AZURE Ad powershell 모듈 개요](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0)를 참조 하세요.

응용 프로그램 프록시에 대 한 다른 PowerShell 예제는 azure [AD 응용 프로그램 프록시에 대 한 AZURE AD PowerShell 예제](../application-proxy-powershell-samples.md)를 참조 하세요.