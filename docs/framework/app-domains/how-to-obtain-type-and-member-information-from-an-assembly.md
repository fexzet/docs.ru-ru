---
title: Практическое руководство. Получение сведений о типах и членах из сборки
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- obtaining assembly information
- assemblies [.NET Framework], obtaining information from
ms.assetid: 348ae651-ccda-4f13-8eda-19e8337e9438
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 44fcded298f33a420a505900f618c1c67f4b9b6f
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2018
ms.locfileid: "50180066"
---
# <a name="how-to-obtain-type-and-member-information-from-an-assembly"></a>Практическое руководство. Получение сведений о типах и членах из сборки
Пространство имен <xref:System.Reflection> содержит много методов для получения сведений из сборки. В этом разделе демонстрируется один из них. Дополнительные сведения см. в разделе [Общие сведения об отражении](../../../docs/framework/reflection-and-codedom/reflection.md).  
  
 В следующем примере демонстрируется получение сведений о типах и членах из сборки.  
  
## <a name="example"></a>Пример  
 [!code-cpp[Conceptual.Types.ViewInfo#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.viewinfo/cpp/source6.cpp#8)]
 [!code-csharp[Conceptual.Types.ViewInfo#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.viewinfo/cs/source6.cs#8)]
 [!code-vb[Conceptual.Types.ViewInfo#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.viewinfo/vb/source6.vb#8)]  
  
## <a name="see-also"></a>См. также  
- [Программирование с использованием доменов приложений](./application-domains.md#programming-with-application-domains)
- [Отражение](../../../docs/framework/reflection-and-codedom/reflection.md)  
- [Использование доменов приложений](../../../docs/framework/app-domains/use.md)
