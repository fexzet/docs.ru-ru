---
title: Практическое руководство. Анимация 3-D вращения с помощью опорных кадров
ms.date: 03/30/2017
helpviewer_keywords:
- animation [WPF], 3-D translations [WPF], with key frames (Rotation3DAnimation)
- key frames [WPF], Rotation3DAnimation
- 3-D translations [WPF], animating [WPF], with key frames (Rotation3DAnimation)
ms.assetid: 6f671b95-7f30-4836-9a4f-aeb7dc30121f
ms.openlocfilehash: 085b2da20410d53fce6099131bf07249bde3209c
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33557628"
---
# <a name="how-to-animate-a-3-d-rotation-using-key-frames"></a>Практическое руководство. Анимация 3-D вращения с помощью опорных кадров
В следующем примере <xref:System.Windows.Media.Animation.Rotation3DAnimationUsingKeyFrames> используются, чтобы сделать трехмерный объект вращаться во время его ось вращения анимирует, что приводит к «Иванин». Эта анимация использует следующие ключевые кадры:  
  
1.  <xref:System.Windows.Media.Animation.LinearRotation3DKeyFrame> используется для создания гладкой, линейной интерполяции между значениями.  
  
2.  <xref:System.Windows.Media.Animation.DiscreteRotation3DKeyFrame> используется для создания резкие «переходы» между значениями (без интерполяции).  
  
3.  <xref:System.Windows.Media.Animation.SplineRotation3DKeyFrame> используется для создания переменного перехода между значениями в зависимости от <xref:System.Windows.Media.Animation.SplineRotation3DKeyFrame.KeySpline%2A> свойство. В приведенном ниже примере эта часть анимации начинается медленно, но ближе к концу временного отрезка, экспоненциально ускоряется.  
  
## <a name="example"></a>Пример  
 [!code-xaml[Animation3DGallery_snip#Rotation3DAnimationUsingKeyFramesExampleWholePage](../../../../samples/snippets/csharp/VS_Snippets_Wpf/Animation3DGallery_snip/CS/Rotation3DAnimationUsingKeyFramesExample.xaml#rotation3danimationusingkeyframesexamplewholepage)]  
  
## <a name="see-also"></a>См. также  
 [Обзор трехмерной графики](../../../../docs/framework/wpf/graphics-multimedia/3-d-graphics-overview.md)  
 [Общие сведения об анимации по ключевым кадрам](../../../../docs/framework/wpf/graphics-multimedia/key-frame-animations-overview.md)  
 [Анимация трехмерного вращения с помощью раскадровки](../../../../docs/framework/wpf/graphics-multimedia/how-to-animate-a-3-d-rotation-using-storyboards.md)  
 [Анимация трехмерного поворота с помощью Rotation3DAnimation](../../../../docs/framework/wpf/graphics-multimedia/how-to-animate-a-3-d-rotation-using-rotation3danimation.md)  
 [Анимация трехмерного вращения с помощью кватернионов](../../../../docs/framework/wpf/graphics-multimedia/how-to-animate-a-3-d-rotation-using-quaternions.md)  
 [Анимация трехмерного поворота с помощью ключевых кадров (QuaternionAnimationUsingKeyFrames)](../../../../docs/framework/wpf/graphics-multimedia/animate-a-3-d-rotation-quaternionanimationusingkeyframes.md)
