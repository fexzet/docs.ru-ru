---
title: Структура COR_ARRAY_LAYOUT
ms.date: 03/30/2017
api_name:
- COR_ARRAY_LAYOUT
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- COR_ARRAY_LAYOUT
helpviewer_keywords:
- COR_DEBUG_IL_TO_NATIVE_MAP structure [.NET Framework debugging]
ms.assetid: aa20ac3d-6f60-4aa2-91c5-f3a86f82eba8
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 7a96542ab5113311bba79cc552afd7f29e6eafa2
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33406400"
---
# <a name="corarraylayout-structure"></a>Структура COR_ARRAY_LAYOUT
Предоставляет сведения о расположении объекта массива в памяти.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
typedef struct COR_ARRAY_LAYOUT {  
    COR_TYPEID componentID;  
    CorElementType componentType;  
    ULONG32 firstElementOffset;  
    ULONG32 elementSize;  
    ULONG32 countOffset;   
    ULONG32 rankSize;   
    ULONG32 numRanks;   
    ULONG32 rankOffset;   
} COR_ARRAY_LAYOUT;  
```  
  
## <a name="members"></a>Участники  
  
|Член|Описание|  
|------------|-----------------|  
|`componentID`|Идентификатор типа объектов, содержащихся в массиве.|  
|`componentType`|Значение перечисления CorElementType, указывающее, является ли компонент ссылка на сборку мусора, класс значений или простому типу.|  
|`firstElementOffset`|Смещение для первого элемента в массиве.|  
|`elementSize`|Размер каждого элемента.|  
|`countOffset`|Смещение, равное количеству элементов в массиве.|  
|`rankSize`|Размер ранг в байтах.|  
|`numRanks`|Число ранги в массиве.|  
|`rankOffset`|Смещение, с которой начинается ранги.|  
  
## <a name="remarks"></a>Примечания  
 `rankSize` Поле указывает размер ранг в многомерный массив. Он является точным для также одномерные массивы.  
  
 Значение `numRanks` -1 для одномерного массива и `N` для многомерный массив `N` измерения.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** CorDebug.idl, CorDebug.h  
  
 **Библиотека:** CorGuids.lib  
  
 **Версии платформы .NET framework:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Структуры отладки](../../../../docs/framework/unmanaged-api/debugging/debugging-structures.md)  
 [Отладка](../../../../docs/framework/unmanaged-api/debugging/index.md)
