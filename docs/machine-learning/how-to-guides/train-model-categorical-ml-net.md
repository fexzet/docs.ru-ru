---
title: Конструирование признаков для обучения модели по текстовым данным — ML.NET
description: Узнайте о том, как конструировать признаки для обучения модели машинного обучения по категориальным данным с помощью ML.NET
ms.date: 11/07/2018
ms.custom: mvc,how-to
ms.openlocfilehash: 10441ed4bfad4cccc51ebbf589ef313d1fe53f6e
ms.sourcegitcommit: ccd8c36b0d74d99291d41aceb14cf98d74dc9d2b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2018
ms.locfileid: "53156612"
---
# <a name="apply-feature-engineering-for-model-training-on-categorical-data---mlnet"></a>Конструирование признаков для обучения модели по текстовым данным — ML.NET

Вам нужно преобразовать все данные, не связанные с числами с плавающей запятой, в типы данных `float`, так как все `learners` ML.NET ожидают признаки в виде `float vector`.

Если набор данных содержит данные `categorical` (например, enum), в ML.NET их можно преобразовать в признаки несколькими способами:

- прямое кодирование;
- прямое кодирование на основе хэша;
- двоичное кодирование (с преобразованием индекса категории в двоичную последовательность и использованием битов как признаков).

Объект `one-hot encoding` может быть избыточным, если некоторые категории имеют большую кратность (часто встречается множество разных значений с небольшими наборами). В таком случае нужно сократить число слотов для кодирования, выбирая признаки на основе количества.

Включите добавление категориальных признаков непосредственно в конвейере обучения ML.NET, чтобы убедиться, что категориальное преобразование:

- обучалось только на данных для обучения, а не на тестовых данных;
- правильно применялось для новых входящих данных без дополнительной предобработки во время прогнозирования.

В следующем примере демонстрируется категориальная обработка для [набора данных учета численности взрослого населения](https://github.com/dotnet/machinelearning/blob/master/test/data/adult.tiny.with-schema.txt):

```console
Label   Workclass   education   marital-status  occupation  relationship    ethnicity   sex native-country-region   age fnlwgt  education-num   capital-gain    capital-loss    hours-per-week
0   Private 11th    Never-married   Machine-op-inspct   Own-child   Black   Male    United-States   25  226802  7   0   0   40
0   Private HS-grad Married-civ-spouse  Farming-fishing Husband White   Male    United-States   38  89814   9   0   0   50
1   Local-gov   Assoc-acdm  Married-civ-spouse  Protective-serv Husband White   Male    United-States   28  336951  12  0   0   40
1   Private Some-college    Married-civ-spouse  Machine-op-inspct   Husband Black   Male    United-States   44  160323  10  7688    0   40
```

```csharp
// Create a new context for ML.NET operations. It can be used for exception tracking and logging, 
// as a catalog of available operations and as the source of randomness.
var mlContext = new MLContext();

// Define the reader: specify the data columns and where to find them in the text file.
var reader = mlContext.Data.TextReader(new TextLoader.Arguments
{
    Column = new[] {
        new TextLoader.Column("Label", DataKind.BL, 0),
        // We will load all the categorical features into one vector column of size 8.
        new TextLoader.Column("CategoricalFeatures", DataKind.TX, 1, 8),
        // Similarly, load all numerical features into one vector of size 6.
        new TextLoader.Column("NumericalFeatures", DataKind.R4, 9, 14),
        // Let's also separately load the 'Workclass' column.
        new TextLoader.Column("Workclass", DataKind.TX, 1),
    },
    HasHeader = true
});

// Read the data.
var data = reader.Read(dataPath);

// Inspect the first 10 records of the categorical columns to check that they are correctly read.
var catColumns = data.GetColumn<string[]>(mlContext, "CategoricalFeatures").Take(10).ToArray();

// Build several alternative featurization pipelines.
var dynamicPipeline =
    // Convert each categorical feature into one-hot encoding independently.
    mlContext.Transforms.Categorical.OneHotEncoding("CategoricalFeatures", "CategoricalOneHot")
    // Convert all categorical features into indices, and build a 'word bag' of these.
    .Append(mlContext.Transforms.Categorical.OneHotEncoding("CategoricalFeatures", "CategoricalBag", CategoricalTransform.OutputKind.Bag))
    // One-hot encode the workclass column, then drop all the categories that have fewer than 10 instances in the train set.
    .Append(mlContext.Transforms.Categorical.OneHotEncoding("Workclass", "WorkclassOneHot"))
    .Append(new CountFeatureSelector(mlContext, "WorkclassOneHot", "WorkclassOneHotTrimmed", count: 10));

// Let's train our pipeline, and then apply it to the same data.
var transformedData = dynamicPipeline.Fit(data).Transform(data);

// Inspect some columns of the resulting dataset.
var categoricalBags = transformedData.GetColumn<float[]>(mlContext, "CategoricalBag").Take(10).ToArray();
var workclasses = transformedData.GetColumn<float[]>(mlContext, "WorkclassOneHotTrimmed").Take(10).ToArray();

// Of course, if we want to train the model, we will need to compose a single float vector of all the features.
// Here's how we could do this:

var fullLearningPipeline = dynamicPipeline
    // Concatenate two of the 3 categorical pipelines, and the numeric features.
    .Append(mlContext.Transforms.Concatenate("Features", "NumericalFeatures", "CategoricalBag", "WorkclassOneHotTrimmed"))
    // Now we're ready to train. We chose our FastTree trainer for this classification task.
    .Append(mlContext.BinaryClassification.Trainers.FastTree(numTrees: 50));

// Train the model.
var model = fullLearningPipeline.Fit(data);
```
