---
title: Защищенные члены
ms.date: 10/22/2008
ms.technology: dotnet-standard
helpviewer_keywords:
- members [.NET Framework], protected
- protected members
- classes [.NET Framework], unsealed
- classes [.NET Framework], protected members
- unsealed classes
- customizing class behavior
ms.assetid: aa0b58ee-3956-494d-ab48-471ae5db8740
author: KrzysztofCwalina
ms.openlocfilehash: f0ad21f0a5b869332223d96991dd0a7bebeba420
ms.sourcegitcommit: ccd8c36b0d74d99291d41aceb14cf98d74dc9d2b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2018
ms.locfileid: "53149358"
---
# <a name="protected-members"></a>Защищенные члены
Защищенные члены сами по себе не предоставляют все расширения, но они могут сделать расширяемости через подклассы более мощные. Они могут использоваться для предоставления Дополнительные параметры настройки без усложнять основной открытый интерфейс.  
  
 Конструкторы Framework нужно очень осторожно защищенные члены, поскольку имя «защищен» можно предоставить ложное чувство защищенности. Любой пользователь может подкласс Незапечатанный класс и доступ к защищенным членам, и поэтому те же защитных приемы программирования, используемый для открытых членов применяются к защищенным членам.  
  
 **✓ CONSIDER** с помощью защищенные члены для расширенной настройки.  
  
 **✓ DO** обрабатывать защищенные члены незапечатанных классов как открытые с целью анализа безопасности, документацию и совместимости.  
  
 Любой пользователь может наследовать от класса и доступа к защищенным членам.  
  
 *Фрагменты: © Корпорация Майкрософт (Microsoft Corporation), 2005, 2009. Все права защищены.*  
  
 *Перепечатано разрешением Пирсона для образовательных учреждений, Inc. из [рекомендации по разработке Framework: Условные обозначения, стили и шаблоны для библиотеки .NET для повторного использования, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) Кшиштов Квалина и Брэд Абрамс, опубликованных 22 октября 2008 г., издательство Addison-Wesley Professional как части цикла разработки Microsoft Windows.*  
  
## <a name="see-also"></a>См. также

- [Рекомендации по проектированию на основе Framework](../../../docs/standard/design-guidelines/index.md)  
- [Разработка с обеспечением расширяемости](../../../docs/standard/design-guidelines/designing-for-extensibility.md)
