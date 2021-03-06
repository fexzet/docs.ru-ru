---
title: . (Доступ к членам) (язык Entity SQL)
ms.date: 03/30/2017
ms.assetid: 4733e3b2-3efa-4b96-b591-ac31350e96ad
ms.openlocfilehash: fdcd026d245b3f6d6ecaccc0f828f3d77fd6ce1a
ms.sourcegitcommit: 11f11ca6cefe555972b3a5c99729d1a7523d8f50
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32764636"
---
# <a name="-member-access-entity-sql"></a>. (Доступ к членам) (язык Entity SQL)
Точка (.) является [!INCLUDE[esql](../../../../../../includes/esql-md.md)] оператор доступа к члену. Оператор доступа к элементу можно использовать, чтобы выдавать значение свойства или поля экземпляра структурного типа концептуальной модели.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
expression.identifier  
```  
  
## <a name="arguments"></a>Аргументы  
 `expression`  
 Экземпляр структурного типа концептуальной модели.  
  
 `identifier`  
 Свойство или поле, принадлежащее экземпляру объекта.  
  
## <a name="remarks"></a>Примечания  
 Оператор «точка» (.) можно использовать для извлечения полей из записи подобно извлечению свойств сложного типа или типа сущности. Например, если «n» типа Name является элементом типа Name, а «p» является элементом типа Person, то «p.n» будет допустимым выражением доступа к элементу, которое выдаст значение типа Name.  
  
 `select p.Name.FirstName from LOB.Person as p`  
  
## <a name="see-also"></a>См. также  
 [Справочник по Entity SQL](../../../../../../docs/framework/data/adonet/ef/language-reference/entity-sql-reference.md)
