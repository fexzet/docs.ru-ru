---
title: facet
ms.date: 03/30/2017
ms.assetid: 91c4e6aa-3e54-4b6c-a38a-abf27808cc85
ms.openlocfilehash: 2b263a107eae7c75b035dcb4675725556e0f9c2b
ms.sourcegitcommit: 11f11ca6cefe555972b3a5c99729d1a7523d8f50
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32762427"
---
# <a name="facet"></a>facet
Объект *аспекта* используется для добавления сведений в определение свойства примитивного типа. Объект [свойство](../../../../docs/framework/data/adonet/property.md) определение содержит сведения о типе свойства, но часто требуется больше сведений. Например, тип сущности в концептуальной модели может иметь свойство типа `String`, значение которого не может быть равно NULL. Аспекты позволяют задавать такой уровень сведений.  
  
 В следующей таблице описываются аспекты, поддерживаемые в модели EDM.  
  
> [!NOTE]
>  Точные значения и поведения аспектов определяются средой выполнения, использующей реализацию модели EDM.  
  
|Аспект|Описание|Применение|  
|-----------|-----------------|----------------|  
|`Collation`|Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.|`String`|  
|`ConcurrencyMode`|Указывает, что значение свойства должно использоваться в проверках оптимистического управления параллелизмом.|Все свойства примитивного типа|  
|`Default`|Задает значение по умолчанию для свойства в случае, если при создании экземпляра не было задано значение.|Все свойства примитивного типа|  
|`FixedLength`|Указывает, может ли изменяться длина значения свойства.|`Binary`, `String`|  
|`MaxLength`|Указывает максимальную длину значения свойства.|`Binary`, `String`|  
|`Nullable`|Задает, может ли свойство принимать значение NULL.|Все свойства примитивного типа|  
|`Precision`|Для свойств типа `Decimal` задается число цифр, которое может иметь значение свойства. Для свойств типа `Time`, `DateTime` и `DateTimeOffset` задается число цифр для части долей секунды значения свойства.|`DateTime`, `DateTimeOffset`, `Decimal`, `Time`,|  
|`Scale`|Задает число цифр справа от десятичной запятой в значении свойства.|Десятичное число|  
|`Unicode`|Указывает, будет ли значение свойства храниться в Юникоде.|`String`|  
  
## <a name="example"></a>Пример  
 [ADO.NET Entity Framework](../../../../docs/framework/data/adonet/ef/index.md) использует доменный язык (DSL), называемый языком определения концептуальной схемы ([CSDL](../../../../docs/framework/data/adonet/ef/language-reference/csdl-specification.md)) для определения концептуальных моделей. Далее на языке CSDL определяется тип сущности `Book`. Обратите внимание, что аспекты реализуются как атрибуты XML. Значения аспекта указывают, что свойство не может быть задано со значением NULL и что свойства `Scale` и `Precision``Revision` имеют каждый значение 29.  
  
 [!code-xml[EDM_Example_Model#EntityExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entityexample)]  
  
## <a name="see-also"></a>См. также  
 [Основные понятия модели EDM](../../../../docs/framework/data/adonet/entity-data-model-key-concepts.md)  
 [Сущностная модель данных](../../../../docs/framework/data/adonet/entity-data-model.md)
