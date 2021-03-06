---
title: Azure Service Bus 릴레이에 대 한 보안 제어
description: 이 문서에서는 Azure Service Bus 릴레이를 평가 하기 위한 기본 제공 보안 컨트롤의 검사 목록을 제공 합니다.
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 28d3ba14aa7769ac4f3fc22bd2b5bd7acd30557c
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514020"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Azure Service Bus 릴레이에 대 한 보안 제어

이 문서에서는 Azure Service Bus Relay에 기본 제공 되는 보안 컨트롤을 설명 합니다.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>네트워크

| 보안 제어 | 예/아니요 | 메모 | 설명서 |
|---|---|--|--|
| 서비스 엔드포인트 지원| 아닙니다. |  |   |
| 네트워크 격리 및 방화벽 지원| 아닙니다. |  |   |
| 강제 터널링 지원| N/A | 릴레이가 TLS 터널  |   |

## <a name="monitoring--logging"></a>& 로깅 모니터링

| 보안 제어 | 예/아니요 | 메모| 설명서 |
|---|---|--|--|
| Azure 모니터링 지원 (Log analytics, App insights 등)| 예 | |   |
| 제어 및 관리 평면 로깅 및 감사| 예 | [Azure Resource Manager](../azure-resource-manager/index.yml). |   |
| 데이터 평면 로깅 및 감사| 예 | 연결 성공/실패 및 오류 및 기록 됨  |   |

## <a name="identity"></a>ID

| 보안 제어 | 예/아니요 | 메모| 설명서 |
|---|---|--|--|
| 인증| 예 | SAS를 통해. | [Azure Relay 인증 및 권한 부여](relay-authentication-and-authorization.md) |
| 권한 부여|  예 | SAS를 통해. | [Azure Relay 인증 및 권한 부여](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>데이터 보호

| 보안 제어 | 예/아니요 | 메모 | 설명서 |
|---|---|--|--|
| 미사용 서버 쪽 암호화: Microsoft 관리 키 |  N/A | 릴레이는 웹 소켓 이며 데이터를 유지 하지 않습니다. |   |
| 미사용 서버 쪽 암호화: 고객 관리 키 (BYOK) | 아닙니다. | Microsoft TLS 인증서만 사용 합니다.  |   |
| 열 수준 암호화 (Azure Data Services)| N/A | |   |
| 전송 중 암호화 (예: Express 경로 암호화, VNet 암호화 및 VNet-VNet 암호화)| 예 | 서비스에는 TLS가 필요 합니다. |   |
| API 호출 암호화| 예 | Http. |


## <a name="configuration-management"></a>Configuration Management

| 보안 제어 | 예/아니요 | 메모| 설명서 |
|---|---|--|--|
| 구성 관리 지원 (구성의 버전 관리 등)| 예 | [Azure Resource Manager](../azure-resource-manager/index.yml).|   |

## <a name="next-steps"></a>다음 단계

- [Azure 서비스에서 기본 제공 되는 보안 컨트롤](../security/fundamentals/security-controls.md)에 대해 자세히 알아보세요.