---
title: '&lt;Очистить&gt; элемент для &lt;namedCaches&gt;'
ms.date: 03/30/2017
helpviewer_keywords:
- <clear> element for <namedCaches>
- clear element for <namedCaches>
ms.assetid: ea01a858-65da-4348-800f-5e3df59d4d79
author: mcleblanc
ms.author: markl
ms.openlocfilehash: dd521e3afa7584cadb28829028a5ecfd1cb55a92
ms.sourcegitcommit: fa38fe76abdc8972e37138fcb4dfdb3502ac5394
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53612307"
---
# <a name="ltcleargt-element-for-ltnamedcachesgt"></a>&lt;Очистить&gt; элемент для &lt;namedCaches&gt;
Очищает все `namedCache` записей в `namedCaches` коллекции для кэша памяти.  
  
 \<System.Runtime.Caching >  
\<memoryCache >  
\<namedCaches >  
\<add>  
  
## <a name="syntax"></a>Синтаксис  
  
```xml  
<namedCaches>  
    <clear name="default" />  
    <!-- child elements -->  
 </namedCaches>  
```  
  
## <a name="type"></a>Тип  
 `Type`  
  
## <a name="attributes-and-elements"></a>Атрибуты и элементы  
 В следующих разделах описаны атрибуты, дочерние и родительские элементы.  
  
### <a name="attributes"></a>Атрибуты  
 `None`  
  
### <a name="child-elements"></a>Дочерние элементы  
 `None`  
  
### <a name="parent-elements"></a>Родительские элементы  
  
|Элемент|Описание:|  
|-------------|-----------------|  
|[\<namedCaches>](../../../../../docs/framework/configure-apps/file-schema/runtime/namedcaches-element-cache-settings.md)|Содержит коллекцию параметров конфигурации для именованного <xref:System.Runtime.Caching.MemoryCache> экземпляров.|  
  
## <a name="remarks"></a>Примечания  
 `clear` Элемент очищает все `namedCache` записей в коллекции именованных кэшей для кэша памяти. Можно использовать `clear` элемент, прежде чем использовать `add` элемент, чтобы добавить новую запись именованного кэша, чтобы быть уверенным, никакие другие именованные кэши в коллекции.  
  
## <a name="see-also"></a>См. также  
- [\<namedCaches > (параметры кэша)](../../../../../docs/framework/configure-apps/file-schema/runtime/namedcaches-element-cache-settings.md)
