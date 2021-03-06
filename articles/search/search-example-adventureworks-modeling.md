---
title: '예: AdventureWorks Inventory 데이터베이스 모델링'
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search에서 인덱싱 및 전체 텍스트 검색을 위해 관계형 데이터를 모델링 하 고 평면화 된 데이터 집합으로 변환 하는 방법에 대해 알아봅니다.
author: HeidiSteen
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: heidist
ms.openlocfilehash: edb6162724938962df8a7340afea6e930a0b1049
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793002"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-cognitive-search"></a>예: Azure Cognitive Search에 대 한 AdventureWorks 인벤토리 데이터베이스 모델링

Azure Cognitive Search는 일반 행 집합을 [인덱싱 (데이터 수집) 파이프라인](search-what-is-an-index.md)에 대 한 입력으로 받아들입니다. 원본 데이터가 SQL Server 관계형 데이터베이스에서 시작 된 경우이 문서에서는 AdventureWorks 예제 데이터베이스를 예로 사용 하 여 인덱싱 전에 일반 행 집합을 만드는 한 가지 방법을 보여 줍니다.

## <a name="about-adventureworks"></a>AdventureWorks 정보

SQL Server 인스턴스를 사용 하는 경우 [AdventureWorks 예제 데이터베이스](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017)에 대해 잘 알고 있을 수 있습니다. 이 데이터베이스에는 제품 정보를 공개하는 5개 테이블이 포함되어 있습니다.

+ **ProductModel**: 이름
+ **Product**: 이름, 색, 비용, 크기, 무게, 이미지, 범주(각 행은 특정 ProductModel에 조인)
+ **ProductDescription**: 설명
+ **ProductModelProductDescription**: 로캘(각 행은 ProductModel을 특정 언어의 특정 ProductDescription에 조인)
+ **ProductCategory**: 이름, 부모 범주

이 예의 목표는이 모든 데이터를 검색 인덱스에 수집 수 있는 일반 행 집합으로 결합 하는 것입니다. 

## <a name="considering-our-options"></a>옵션 고려

Product 테이블에는 가장 구체적인 정보가 포함되어 있으므로 기본 방법은 Product 테이블의 모든 행을 인덱싱(적절한 경우 조인)하는 것입니다. 그러나 이 방법은 검색 인덱스를 결과 집합의 인지된 중복 요소에 공개합니다. 예를 들어 Road-650 모델은 2가지 색과 6가지 크기가 제공됩니다. "로드 바이크"에 대한 쿼리는 동일한 모델의 12개 인스턴스가 주를 이루며, 크기와 색으로만 구분됩니다. 다른 6개의 로드 관련 모델은 검색의 하위 수준인 2페이지로 강등됩니다.

  ![제품 목록](./media/search-example-adventureworks/products-list.png "제품 목록")
 
Road-650 모델은 12가지 옵션이 있습니다. 일대다 엔터티 행을 가장 잘 나타내는 것은 검색 인덱스의 다중 값 필드 또는 사전 집계 값 필드입니다.

이 문제는 대상 인덱스를 ProductModel 테이블로 이동하는 간단한 방법으로 해결되지 않습니다. 이렇게 하면 검색 결과에 계속 표시되어야 하는 Product 테이블의 중요한 데이터가 무시됩니다.

## <a name="use-a-collection-data-type"></a>컬렉션 데이터 형식 사용

"올바른 방법"은 데이터베이스 모델에서 직접 병렬을 사용 하지 않는 검색 스키마 기능을 활용 하는 것입니다. **Collection (Edm. String)** . 이 구문은 Azure Cognitive Search 인덱스 스키마에 정의 되어 있습니다. 컬렉션 데이터 형식은 매우 긴 (단일) 문자열이 아닌 개별 문자열의 목록을 나타내야 할 때 사용 됩니다. 태그 또는 키워드가 있는 경우 이 필드에 컬렉션 데이터 형식을 사용합니다.

"색", "크기", "이미지"에 대한 **Collection(Edm.String)** 의 다중 값 인덱스 필드를 정의하면 중복 항목으로 인덱스를 오염시키지 않고 패싯 및 필터링을 위한 보조 정보가 보존됩니다. 마찬가지로, 숫자 Product 필드에 집계 함수를 적용하고, 모든 단일 제품 **listPrice** 대신 **minListPrice**를 인덱싱합니다.

이러한 구조를 사용하는 인덱스를 고려할 때 "산악 자전거"를 검색하면 개별 자전거 모델이 표시되고 색, 크기, 최저 가격 등의 중요한 메타데이터가 유지됩니다. 다음 스크린샷은 일러스트레이션을 제공합니다.

  ![산지 자전거 검색 예제](./media/search-example-adventureworks/mountain-bikes-visual.png "산지 자전거 검색 예제")

## <a name="use-script-for-data-manipulation"></a>데이터 조작에 스크립트 사용

하지만 이 유형의 모델링은 SQL 문 하나로는 쉽게 수행할 수 없습니다. 그 대신, 간단한 NodeJS 스크립트를 사용하여 데이터를 로드한 다음, 검색 친화적인 JSON 엔터티에 매핑하세요.

최종 데이터베이스 검색 매핑은 다음과 같습니다.

+ model (Edm.String: searchable, filterable, retrievable) from "ProductModel.Name"
+ description_en (Edm.String: searchable) from "ProductDescription" for the model where culture=’en’
+ color (Collection(Edm.String): searchable, filterable, facetable, retrievable): unique values from "Product.Color" for the model
+ size (Collection(Edm.String): searchable, filterable, facetable, retrievable): unique values from "Product.Size" for the model
+ image (Collection(Edm.String): retrievable): unique values from "Product.ThumbnailPhoto" for the model
+ minStandardCost (Edm.Double: filterable, facetable, sortable, retrievable): aggregate minimum of all "Product.StandardCost" for the model
+ minListPrice (Edm.Double: filterable, facetable, sortable, retrievable): aggregate minimum of all "Product.ListPrice" for the model
+ minWeight (Edm.Double: filterable, facetable, sortable, retrievable): aggregate minimum of all "Product.Weight" for the model
+ products (Collection(Edm.String): searchable, filterable, retrievable): unique values from "Product.Name" for the model

Product가 포함된 ProductModel 테이블을 조인하고 ProductDescription을 조인한 다음, [lodash](https://lodash.com/)(또는 C#의 Linq)를 사용하여 신속하게 결과 집합을 변환합니다.

```javascript
var records = queryYourDatabase();
var models = _(records)
  .groupBy('ModelName')
  .values()
  .map(function(d) {
    return {
      model: _.first(d).ModelName,
      description: _.first(d).Description,
      colors: _(d).pluck('Color').uniq().compact().value(),
      products: _(d).pluck('ProductName').uniq().compact().value(),
      sizes: _(d).pluck('Size').uniq().compact().value(),
      images: _(d).pluck('ThumbnailPhotoFilename').uniq().compact().value(),
      minStandardCost: _(d).pluck('StandardCost').min(),
      maxStandardCost: _(d).pluck('StandardCost').max(),
      minListPrice: _(d).pluck('ListPrice').min(),
      maxListPrice: _(d).pluck('ListPrice').max(),
      minWeight: _(d).pluck('Weight').min(),
      maxWeight: _(d).pluck('Weight').max(),
    };
  })
  .value();
```

그 결과로 얻게 되는 JSON은 다음과 같습니다.

```json
[
  {
    "model": "HL Road Frame",
    "colors": [
      "Black",
      "Red"
    ],
    "products": [
      "HL Road Frame - Black, 58",
      "HL Road Frame - Red, 58",
      "HL Road Frame - Red, 62",
      "HL Road Frame - Red, 44",
      "HL Road Frame - Red, 48",
      "HL Road Frame - Red, 52",
      "HL Road Frame - Red, 56",
      "HL Road Frame - Black, 62",
      "HL Road Frame - Black, 44",
      "HL Road Frame - Black, 48",
      "HL Road Frame - Black, 52"
    ],
    "sizes": [
      "58",
      "62",
      "44",
      "48",
      "52",
      "56"
    ],
    "images": [
      "no_image_available_small.gif"
    ],
    "minStandardCost": 868.6342,
    "maxStandardCost": 1059.31,
    "minListPrice": 1431.5,
    "maxListPrice": 1431.5,
    "minWeight": 961.61,
    "maxWeight": 1043.26
  }
]
```

마지막으로, 다음은 초기 레코드 집합을 반환하는 SQL 쿼리입니다. 저는 [mssql](https://www.npmjs.com/package/mssql) npm 모듈을 사용하여 NodeJS 앱에 데이터를 로드했습니다.

```T-SQL
SELECT
  m.Name as ModelName,
  d.Description,
  p.Name as ProductName,
  p.*
FROM 
  SalesLT.ProductModel m
INNER JOIN 
  SalesLT.ProductModelProductDescription md
  ON m.ProductModelId = md.ProductModelId
INNER JOIN 
  SalesLT.ProductDescription d
  ON md.ProductDescriptionId = d.ProductDescriptionId
LEFT JOIN 
  SalesLT.product p
  ON m.ProductModelId = p.ProductModelId
WHERE
  md.Culture='en'
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [예: Azure Cognitive Search의 다중 수준 패싯 분류](search-example-adventureworks-multilevel-faceting.md)