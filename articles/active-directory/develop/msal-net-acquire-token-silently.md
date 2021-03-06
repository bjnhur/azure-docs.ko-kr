---
title: 캐시에서 토큰 획득 (MSAL.NET)
titleSuffix: Microsoft identity platform
description: Microsoft Authentication Library for .NET (MSAL.NET)을 사용 하 여 토큰 캐시에서 자동으로 액세스 토큰을 획득 하는 방법에 대해 알아봅니다.
services: active-directory
author: TylerMSFT
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: bb17ebf5afdc20eaa9db8b86a5643ad18a112eab
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76695656"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>MSAL.NET를 사용 하 여 토큰 캐시에서 토큰 가져오기

Microsoft Authentication Library for .NET (MSAL.NET)을 사용 하 여 액세스 토큰을 획득 하면 토큰이 캐시 됩니다. 응용 프로그램에 토큰이 필요한 경우 먼저 `AcquireTokenSilent` 메서드를 호출 하 여 허용 되는 토큰이 캐시에 있는지 확인 해야 합니다. 대부분의 경우 캐시의 토큰을 기반으로 더 많은 범위의 다른 토큰을 얻을 수 있습니다. 토큰 캐시에도 새로 고침 토큰이 포함 되어 있으므로 만료에 가까운 경우에도 토큰을 새로 고칠 수 있습니다.

권장 되는 패턴은 `AcquireTokenSilent` 메서드를 먼저 호출 하는 것입니다.  `AcquireTokenSilent` 실패 하면 다른 방법을 사용 하 여 토큰을 획득 합니다.

다음 예제에서 응용 프로그램은 먼저 토큰 캐시에서 토큰을 가져오려고 시도 합니다.  `MsalUiRequiredException` 예외가 throw 되 면 응용 프로그램은 토큰을 대화형으로 가져옵니다. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilent.
 // This indicates you need to call AcquireTokenInteractive to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```
