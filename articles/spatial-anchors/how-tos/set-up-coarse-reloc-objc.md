---
title: Objective-C의 광역 위치 재결정
description: Objective-C의 광역 위치 재결정을 사용하여 앵커를 만들고 찾는 방법에 대해 자세히 설명합니다.
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.author: bobuc
ms.date: 09/19/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 251f0d8609921bd1d0222d9e30c537ecbb2a04bd
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548274"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-objective-c"></a>Objective-C의 광역 위치 재결정을 사용하여 앵커를 만들고 찾는 방법

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android Java](set-up-coarse-reloc-java.md)
> * [C++/NDK](set-up-coarse-reloc-cpp-ndk.md)
> * [C++/WinRT](set-up-coarse-reloc-cpp-winrt.md)

Azure Spatial Anchors는 사용자가 만든 앵커와 디바이스 내 위치 센서 데이터를 연결할 수 있습니다. 이 데이터를 사용하여 디바이스 근처에 앵커가 있는지 여부를 신속하게 확인할 수도 있습니다. 자세한 내용은 [광역 위치 재결정](../concepts/coarse-reloc.md)을 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- Objective-C에 대한 기본 지식.
- [Azure Spatial Anchors 개요](../overview.md)를 자세히 읽었습니다.
- [5분 빠른 시작](../index.yml) 중 하나를 완료했습니다.
- [앵커 만들기 및 찾기 방법](../create-locate-anchors-overview.md)을 통해 읽습니다.

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```objc
// Create the sensor fingerprint provider
ASAPlatformLocationProvider *sensorProvider;
sensorProvider = [[ASAPlatformLocationProvider alloc] init];

// Allow GPS
ASASensorCapabilities *sensors = locationProvider.sensors;
sensors.geoLocationEnabled = true;

// Allow WiFi scanning
sensors.wifiEnabled = true;

// Populate the set of known BLE beacons' UUIDs
NSArray *uuids = @[@"22e38f1a-c1b3-452b-b5ce-fdb0f39535c1", @"a63819b9-8b7b-436d-88ec-ea5d8db2acb0"];

// Allow a set of known BLE beacons
sensors.bluetoothEnabled = true;
sensors.knownBeaconProximityUuids = uuids;
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```objc
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession.locationProvider = sensorProvider;

// Configure the near-device criteria
ASANearDeviceCriteria *nearDeviceCriteria = [[ASANearDeviceCriteria alloc] init];
nearDeviceCriteria.distanceInMeters = 5.0f;
nearDeviceCriteria.maxResultCount = 25;

// Set the session's locate criteria
ASAAnchorLocateCriteria *anchorLocateCriteria = [[ASAAnchorLocateCriteria alloc] init];
anchorLocateCriteria.nearDevice = nearDeviceCriteria;
[cloudSpatialAnchorSession createWatcher:anchorLocateCriteria];
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]