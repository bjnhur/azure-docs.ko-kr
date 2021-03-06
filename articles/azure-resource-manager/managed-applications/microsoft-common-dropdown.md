---
title: DropDown UI 요소
description: Azure Portal의 Microsoft.Common.DropDown UI 요소에 대해 설명합니다. 관리 되는 응용 프로그램을 배포할 때 사용 가능한 옵션 중에서 선택 하는 데 사용 합니다.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: a09f9695c18f368a585dbcd0d1e654dee4adfa03
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75652386"
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown UI 요소

드롭다운 목록을 포함하는 선택 컨트롤입니다.

## <a name="ui-sample"></a>UI 샘플

![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>스키마

```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Example drop down",
  "defaultValue": "Value two",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Value one",
        "value": "one"
      },
      {
        "label": "Value two",
        "value": "two"
      }
    ],
    "required": true
  },
  "visible": true
}
```

## <a name="sample-output"></a>샘플 출력

```json
"two"
```

## <a name="remarks"></a>설명

- `constraints.allowedValues`의 레이블은 항목에 대한 표시 텍스트이며, 해당 값은 선택한 요소의 출력 값입니다.
- 이 값을 지정하면 기본값은 `constraints.allowedValues`에 있는 레이블이어야 합니다. 지정하지 않으면 `constraints.allowedValues`의 첫 번째 항목이 선택됩니다. 기본값은 **null**입니다.
- `constraints.allowedValues`에는 적어도 하나의 항목이 합니다.
- 필요하지 않은 값을 에뮬레이트하려면 레이블과 값이 `""`(빈 문자열)인 항목을 `constraints.allowedValues`에 추가합니다.

## <a name="next-steps"></a>다음 단계

* UI 정의 만들기에 대한 소개는 [CreateUiDefinition 시작](create-uidefinition-overview.md)을 참조하세요.
* UI 요소의 공용 속성에 대한 설명은 [CreateUiDefinition 요소](create-uidefinition-elements.md)를 참조하세요.
