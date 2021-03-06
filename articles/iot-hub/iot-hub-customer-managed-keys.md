---
title: 고객이 관리 하는 키를 통해 미사용 데이터 암호화 Azure IoT Hub | Microsoft Docs
description: IoT Hub를 위해 고객이 관리 하는 키를 사용 하 여 미사용 데이터 암호화
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: asrastog
ms.openlocfilehash: 1bb55d593878026bb3e57014a317b4fc0158d734
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75913135"
---
# <a name="encryption-of-data-at-rest-with-customer-managed-keys-for-iot-hub"></a>IoT Hub를 위해 고객이 관리 하는 키를 사용 하 여 미사용 데이터 암호화

IoT Hub은 사용자가 직접 키 (BYOK)를 사용 하 여 Azure IoT Hub에 대 한 지원을 제공 하는 CMK (고객 관리 키)를 사용 하 여 미사용 데이터의 암호화를 지원 합니다. Azure IoT Hub는 미사용 데이터 및 전송 중인 데이터의 암호화를 제공 합니다. 기본적으로 IoT Hub는 Microsoft 관리 키를 사용 하 여 데이터를 암호화 합니다. CMK 지원을 통해 고객은 이제 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)를 사용 하 여 고객이 관리 하는 키 암호화 키를 사용 하 여 미사용 데이터를 암호화 하도록 선택할 수 있습니다.

이 기능을 사용 하려면 미국 동부, 미국 서 부 2 또는 남부 중부 미국 중 한 곳에 새 IoT Hub (기본 또는 표준 계층)을 만들어야 합니다. 이 기능을 사용해 보려면 [Microsoft 지원](https://azure.microsoft.com/support/create-ticket/)에 문의 하세요. Microsoft 지원에 문의할 때 회사 이름 및 구독 ID를 공유 하세요.

## <a name="next-steps"></a>다음 단계

* [IoT Hub에 대 한 자세한 정보](https://docs.microsoft.com/azure/iot-hub/about-iot-hub)

* [Azure Key Vault에 대 한 자세한 정보](https://docs.microsoft.com/azure/key-vault/key-vault-overview)