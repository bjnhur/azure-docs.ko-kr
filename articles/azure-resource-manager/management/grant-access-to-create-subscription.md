---
title: Azure 엔터프라이즈 구독 만들기에 대 한 액세스 권한 부여
description: Azure 엔터프라이즈 구독을 프로그래밍 방식으로 만드는 기능을 사용자 또는 서비스 주체에 부여하는 방법에 대해 알아봅니다.
author: jureid
manager: jureid
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: jureid
ms.openlocfilehash: b77efd7e5cf7ff016605e0ba2e74cff9ea8dab89
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75478878"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure 엔터프라이즈 구독 만들기에 대한 액세스 권한 부여(미리 보기)

[EA(기업계약)](https://azure.microsoft.com/pricing/enterprise-agreement/)의 Azure 고객은 고객의 계정으로 청구되는 구독을 만들 수 있는 권한을 다른 사용자 또는 서비스 주체에게 부여할 수 있습니다. 이 문서에서는 [RBAC(역할 기반 액세스 제어)](../../active-directory/role-based-access-control-configure.md)를 사용하여 구독을 만드는 기능을 공유하고 구독 생성을 감사하는 방법을 배웁니다. 공유하려는 계정에 대해 소유자 역할이 있어야 합니다.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="grant-access"></a>액세스 권한 부여

[등록 계정에서 구독을 만들려면](programmatically-create-subscription.md)사용자에 게 해당 계정에 대 한 [RBAC 소유자 역할이](../../role-based-access-control/built-in-roles.md#owner) 있어야 합니다. 다음 단계를 수행 하 여 사용자 또는 사용자 그룹에 게 등록 계정에 대 한 RBAC 소유자 역할을 부여할 수 있습니다.

1. 액세스 권한을 부여 하려는 등록 계정의 개체 ID를 가져옵니다.

    다른 사용자에 게 등록 계정에 대 한 RBAC 소유자 역할을 부여 하려면 계정 소유자 또는 계정에 대 한 RBAC 소유자 여야 합니다.

    # <a name="resttabrest"></a>[REST (영문)](#tab/rest)

    액세스 권한이 있는 모든 등록 계정을 나열 하는 요청:

    ```json
    GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
    ```

    Azure는 액세스할 수 있는 모든 등록 계정의 목록으로 응답합니다.

    ```json
    {
      "value": [
        {
          "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "Microsoft.Billing/enrollmentAccounts",
          "properties": {
            "principalName": "SignUpEngineering@contoso.com"
          }
        },
        {
          "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "Microsoft.Billing/enrollmentAccounts",
          "properties": {
            "principalName": "BillingPlatformTeam@contoso.com"
          }
        }
      ]
    }
    ```

    `principalName` 속성을 사용 하 여 RBAC 소유자에 게 액세스 권한을 부여 하려는 계정을 식별 합니다. 해당 계정의 `name`를 복사 합니다. 예를 들어 RBAC 소유자에 게 SignUpEngineering@contoso.com 등록 계정에 대 한 액세스 권한을 부여 하려는 경우 ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```를 복사 합니다. 등록 계정의 개체 ID입니다. 다음 단계에서 `enrollmentAccountObjectId`로 사용할 수 있도록이 값을 어딘가에 붙여넣습니다.

    # <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

    [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet을 사용하여 액세스 권한이 있는 모든 등록 계정을 나열합니다. **시도** 를 선택 하 여 [Azure Cloud Shell](https://shell.azure.com/)을 엽니다. 코드를 붙여넣으려면 셸 창을 마우스 오른쪽 단추로 클릭 하 고 **붙여넣기**를 선택 합니다.

    ```azurepowershell-interactive
    Get-AzEnrollmentAccount
    ```

    Azure는 사용자가 액세스할 수 있는 등록 계정 목록으로 응답 합니다.

    ```azurepowershell
    ObjectId                               | PrincipalName
    747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
    4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
    ```

    `principalName` 속성을 사용 하 여 RBAC 소유자에 게 액세스 권한을 부여 하려는 계정을 식별 합니다. 해당 계정의 `ObjectId`를 복사 합니다. 예를 들어 RBAC 소유자에 게 SignUpEngineering@contoso.com 등록 계정에 대 한 액세스 권한을 부여 하려는 경우 ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```를 복사 합니다. 다음 단계에서 `enrollmentAccountObjectId`사용할 수 있도록이 개체 ID를 어딘가에 붙여넣습니다.

    # <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

    [az billing enrollment-account list](https://aka.ms/EASubCreationPublicPreviewCLI) 명령을 사용하여 액세스할 수 있는 모든 등록 계정을 나열합니다. **시도** 를 선택 하 여 [Azure Cloud Shell](https://shell.azure.com/)을 엽니다. 코드를 붙여넣으려면 셸 창을 마우스 오른쪽 단추로 클릭 하 고 **붙여넣기**를 선택 합니다.

    ```azurecli-interactive
    az billing enrollment-account list
    ```

    Azure는 사용자가 액세스할 수 있는 등록 계정 목록으로 응답 합니다.

    ```json
    [
      {
        "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "principalName": "SignUpEngineering@contoso.com",
        "type": "Microsoft.Billing/enrollmentAccounts",
      },
      {
        "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "principalName": "BillingPlatformTeam@contoso.com",
        "type": "Microsoft.Billing/enrollmentAccounts",
      }
    ]
    ```

    ---

    `principalName` 속성을 사용 하 여 RBAC 소유자에 게 액세스 권한을 부여 하려는 계정을 식별 합니다. 해당 계정의 `name`를 복사 합니다. 예를 들어 RBAC 소유자에 게 SignUpEngineering@contoso.com 등록 계정에 대 한 액세스 권한을 부여 하려는 경우 ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```를 복사 합니다. 등록 계정의 개체 ID입니다. 다음 단계에서 `enrollmentAccountObjectId`로 사용할 수 있도록이 값을 어딘가에 붙여넣습니다.

1. <a id="userObjectId"></a>RBAC 소유자 역할을 부여 하려는 사용자 또는 그룹의 개체 ID를 가져옵니다.

    1. Azure Portal에서 **Azure Active Directory**를 검색 합니다.
    1. 사용자에 게 액세스 권한을 부여 하려면 왼쪽의 메뉴에서 **사용자** 를 클릭 합니다. 그룹에 대 한 액세스 권한을 부여 하려면 **그룹**을 클릭 합니다.
    1. RBAC 소유자 역할에 지정할 사용자 또는 그룹을 선택 합니다.
    1. 사용자를 선택한 경우에는 프로필 페이지에서 개체 ID를 찾을 수 있습니다. 그룹을 선택한 경우 개체 ID는 개요 페이지에 표시 됩니다. 텍스트 상자의 오른쪽에 있는 아이콘을 클릭 하 여 **ObjectID** 를 복사 합니다. 다음 단계에서 `userObjectId`로 사용할 수 있도록이 위치에 붙여 넣습니다.

1. 사용자 또는 그룹에 등록 계정에 대 한 RBAC 소유자 역할을 부여 합니다.

    처음 두 단계에서 수집한 값을 사용 하 여 사용자 또는 그룹에 등록 계정에 대 한 RBAC 소유자 역할을 부여 합니다.

    # <a name="resttabrest-2"></a>[REST (영문)](#tab/rest-2)

    다음 명령을 실행 하 여 ```<enrollmentAccountObjectId>```을 첫 번째 단계에서 복사한 `name` (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```)로 바꿉니다. ```<userObjectId>```를 두 번째 단계에서 복사한 개체 ID로 바꿉니다.

    ```json
    PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

    {
      "properties": {
        "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
        "principalId": "<userObjectId>"
      }
    }
    ```

    등록 계정 범위에서 소유자 역할을 성공적으로 할당하면 Azure는 역할 할당의 정보로 응답합니다.

    ```json
    {
      "properties": {
        "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
        "principalId": "<userObjectId>",
        "scope": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "createdOn": "2018-03-05T08:36:26.4014813Z",
        "updatedOn": "2018-03-05T08:36:26.4014813Z",
        "createdBy": "<assignerObjectId>",
        "updatedBy": "<assignerObjectId>"
      },
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "<roleAssignmentGuid>"
    }
    ```

    # <a name="powershelltabazure-powershell-2"></a>[PowerShell](#tab/azure-powershell-2)

    다음 [AzRoleAssignment](../../active-directory/role-based-access-control-manage-access-powershell.md) 명령을 실행 하 여 첫 번째 단계 (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```)에서 수집 된 `ObjectId` ```<enrollmentAccountObjectId>```를 바꿉니다. ```<userObjectId>```를 두 번째 단계에서 수집 된 개체 ID로 바꿉니다.

    ```azurepowershell-interactive
    New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    # <a name="azure-clitabazure-cli-2"></a>[Azure CLI](#tab/azure-cli-2)

    다음 [az role 대입문 create](../../active-directory/role-based-access-control-manage-access-azure-cli.md) 명령을 실행 하 고 ```<enrollmentAccountObjectId>```을 첫 번째 단계에서 복사한 `name` (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```)로 바꿉니다. ```<userObjectId>```를 두 번째 단계에서 수집 된 개체 ID로 바꿉니다.

    ```azurecli-interactive
    az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    사용자가 등록 계정에 대 한 RBAC 소유자가 되 면 그 아래에서 [프로그래밍 방식으로 구독을 만들](programmatically-create-subscription.md) 수 있습니다. 위임 된 사용자가 만든 구독은 여전히 원래 계정 소유자가 서비스 관리자로 사용 되지만 위임 된 사용자는 기본적으로 RBAC 소유자로 사용 됩니다.

    ---

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>활동 로그를 사용하여 구독을 만든 사람 감사

이 API를 통해 만든 구독을 추적하려면 [테넌트 활동 로그 API](/rest/api/monitor/tenantactivitylogs)를 사용합니다. 현재 PowerShell, CLI 또는 Azure Portal을 사용하여 구독 만들기를 추적할 수 없습니다.

1. Azure AD 테넌트의 테넌트 관리자로 [액세스의 권한을 상승한](../../active-directory/role-based-access-control-tenant-admin-access.md) 다음, `/providers/microsoft.insights/eventtypes/management` 범위에 대해 감사 사용자에게 읽기 역할을 할당합니다.
1. 감사 사용자로 [테넌트 활동 로그 API](/rest/api/monitor/tenantactivitylogs)를 호출하여 구독 생성 작업을 봅니다. 예:

    ```
    GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'"
    ```

명령줄에서 이 API를 편리하게 호출하려면 [ARMClient](https://github.com/projectkudu/ARMClient)를 시도합니다.

## <a name="next-steps"></a>다음 단계

* 사용자 또는 서비스 주체가 구독을 만들 권한을 가지므로 해당 ID를 사용하여 [프로그래밍 방식으로 Azure 엔터프라이즈 구독을 만들 수 있습니다](programmatically-create-subscription.md).
* .NET을 사용하여 구독 만들기에 대한 예제는 [GitHub의 샘플 코드](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core)를 참조하세요.
* Azure Resource Manager 및 해당 API에 대한 자세한 내용은 [Azure Resource Manager 개요](overview.md)를 참조하세요.
* 관리 그룹을 사용하여 많은 수의 구독 관리에 대해 자세히 알아보려면 [Azure 관리 그룹으로 리소스 구성](../../governance/management-groups/overview.md)을 참조하세요.
* 구독 거버넌스에서 대규모 조직에 대한 포괄적인 모범 사례 지침을 보려면 [Azure 엔터프라이즈 스캐폴드 - 규범적 구독 거버넌스](/azure/architecture/cloud-adoption-guide/subscription-governance)를 참조하세요.
