---
title: Реализация прокси-сервера обнаружения
ms.date: 03/30/2017
ms.assetid: dda20e79-8df3-438e-a281-69d779d978ec
ms.openlocfilehash: 0c7086e0eecea6cc2d7494d6afda0abf056ba758
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33492223"
---
# <a name="implementing-a-discovery-proxy"></a>Реализация прокси-сервера обнаружения
В данном разделе приводятся инструкции по реализации прокси-сервера обнаружения. Прокси-сервер обнаружения - это автономная служба, содержащая репозиторий служб. Клиенты могут выполнять запросы к прокси-серверу обнаружения, чтобы найти обнаружимые службы, доступные серверу. Метод заполнения прокси-сервера службами определяется разработчиком. Например, прокси-сервер обнаружения может подключаться к существующему репозиторию служб и сделать эту информацию обнаружимой, администратор может при помощи API управления добавить обнаружимые службы к прокси-серверу, а прокси-сервер обнаружения может при помощи функции объявлений обновить свой внутренний кэш.  
  
 Реализация WCF обеспечивает базовые классы, которые позволят легко создавать прокси-сервер. Эти API-интерфейсы можно использовать для создания прокси-сервера обнаружения поверх существующего репозитория.  
  
 Используемый здесь прокси-сервер обнаружения похож на любую другую службу WCF в том, что можно сделать прокси-сервер обнаруживаемым и позволить клиентам находить его конечные точки.  
  
## <a name="in-this-section"></a>В этом разделе  
 [Практическое руководство. Реализация прокси-сервера обнаружения](../../../../docs/framework/wcf/feature-details/how-to-implement-a-discovery-proxy.md)  
 Реализация прокси-сервера обнаружения.  
  
 [Практическое руководство. Реализация обнаруживаемой службы, которая регистрируется в прокси-сервере обнаружения](../../../../docs/framework/wcf/feature-details/discoverable-service-that-registers-with-the-discovery-proxy.md)  
 Описывает, как реализовать обнаруживаемой службы WCF, которая регистрирует прокси-сервере обнаружения.  
  
 [Практическое руководство. Реализация клиентского приложения, которое для поиска служб использует прокси-сервер обнаружения](../../../../docs/framework/wcf/feature-details/client-app-discovery-proxy-to-find-a-service.md)  
 Описывает, как реализовать приложение клиента WCF, которое использует прокси-сервер обнаружения для поиска службы.  
  
 [Практическое руководство. Тестирование прокси-сервера обнаружения](../../../../docs/framework/wcf/feature-details/how-to-test-the-discovery-proxy.md)  
 Описывает, как проверить код, приведенный в предыдущих трех разделах.  
  
## <a name="see-also"></a>См. также  
 [Обнаружение WCF](../../../../docs/framework/wcf/feature-details/wcf-discovery.md)  
 [Практическое руководство. Программное добавление возможности обнаружения к службе и клиенту WCF](../../../../docs/framework/wcf/feature-details/how-to-programmatically-add-discoverability-to-a-wcf-service-and-client.md)
