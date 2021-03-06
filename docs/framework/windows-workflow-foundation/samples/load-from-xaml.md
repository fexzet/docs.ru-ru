---
title: Загрузка из XAML
ms.date: 03/30/2017
ms.assetid: 1f103ef6-7bed-4f16-ae52-9e665c5a43d7
ms.openlocfilehash: 783e26b05d23baa7842c4414c92d4e78262dd9ec
ms.sourcegitcommit: 8c28ab17c26bf08abbd004cc37651985c68841b8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2018
ms.locfileid: "48845464"
---
# <a name="load-from-xaml"></a>Загрузка из XAML
В этом образце показан способ динамической загрузки рабочего процесса XAML без применения средства XamlBuildTask. Вместо этого в образце вызывается метод <xref:System.Activities.XamlIntegration.ActivityXamlServices.Load%2A>. Образец представляет клиентское приложение Windows Presentation Foundation (WPF), который загружает рабочие процессы XAML, с помощью <xref:System.Activities.XamlIntegration.ActivityXamlServices> класса и выполняет их. После загрузки этих процессов с использованием класса <xref:System.Activities.XamlIntegration.ActivityXamlServices> возвращается действие <xref:System.Activities.DynamicActivity%601>, которое может быть выполнено.

#### <a name="to-use-this-sample"></a>Использование этого образца

1.  С помощью Visual Studio 2010, откройте решения loadfromxaml.sln.

2.  Для построения решения нажмите CTRL+SHIFT+B.

3.  Чтобы запустить решение, нажмите клавиши CTRL+F5.

> [!IMPORTANT]
>  Образцы уже могут быть установлены на компьютере. Перед продолжением проверьте следующий каталог (по умолчанию).  
>   
>  `<InstallDrive>:\WF_WCF_Samples`  
>   
>  Если этот каталог не существует, перейдите к [Windows Communication Foundation (WCF) и образцы Windows Workflow Foundation (WF) для .NET Framework 4](https://go.microsoft.com/fwlink/?LinkId=150780) для загрузки всех Windows Communication Foundation (WCF) и [!INCLUDE[wf1](../../../../includes/wf1-md.md)] примеры. Этот образец расположен в следующем каталоге.  
>   
>  `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Built-InActivities\DynamicActivity\LoadFromXAML`