---
author: lucygoldbergmicrosoft
ms.service: data-explorer
ms.topic: include
ms.date: 11/28/2019
ms.author: lugoldbe
ms.openlocfilehash: 9598724db630db7fc48545b58f2b0c1cad464aa4
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74667834"
---
## <a name="clean-up-resources"></a>리소스 정리

Azure 리소스가 더 이상 필요하지 않은 경우 리소스 그룹을 삭제하여 배포한 리소스를 정리합니다. 

### <a name="clean-up-resources-using-the-azure-portal"></a>Azure Portal를 사용 하 여 리소스 정리

[리소스 정리](../articles/data-explorer/create-cluster-database-portal.md#clean-up-resources)의 단계에 따라 Azure Portal에서 리소스를 삭제 합니다.

### <a name="clean-up-resources-using-powershell"></a>PowerShell을 사용하여 리소스 정리

Cloud Shell 아직 열려 있으면 첫 번째 줄 (읽기-호스트)을 복사/실행할 필요가 없습니다.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```