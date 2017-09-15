---

# required metadata
title: "Publish, deploy, update, and delete Python web services - Machine Learning Server | Microsoft Docs"
description: "Publish, update, and delete Python web services with Microsoft R Server"
keywords: ""
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "9/25/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology: 
  - deployr
  - r-server
#ms.custom: ""
---

# What are web services in Machine Learning Server?

**Applies to: Machine Learning Server**

To offer other users a chance to use the code and predictive models you have developed, you can deploy them as web service hosted in Machine Learning Server. Once you deploy the web service, it can be consumed by other authenticated users.

Web services in Machine Learning Server can be based on Python or R. They can also be deployed on one platform and consumed in another. 

There are two types of web services: standard and realtime. 

## Requirements

Before you can deploy, manage, or consume web services, you must have access to an instance of Machine Learning Server that was  [properly configured](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) to host web services. 

<a name="standard"></a>

## Standard web services

These web services offer fast execution and scoring of arbitrary Python or R code and models. They can contain code, models, and model assets. They can also take specific inputs and provide specific outputs for those users who are integrating the services inside their applications.

Standard web services, like all web services, are identified by their name and version. Additionally, a standard web service is also defined by any Python or R code, models, and any necessary model assets. When deploying a standard web service, you should also define the required inputs and any output the application developers use to integrate the service in their applications.

**See a standard web service deployment example: [R](../operationalize/quickstart-publish-r-web-service.md)  |  [Python](../operationalize/python/quickstart-deploy-python-web-service.md)**

<a name="realtime"></a>

## Realtime web services

Once you've built a predictive model, in many cases the next step is to operationalize the model. That is to generate predictions from the pre-trained model in real time. In this scenario, where new data often become available one row at a time, latency becomes the critical metric. It is important to respond with the single prediction (or score) as quickly as possible.

Realtime web services offer even lower latency to produce results faster and score more models in parallel. The improved performance boost comes from the fact that these web services do not rely on an interpreter at consumption time even though the services use the objects created by the model. Therefore, fewer additional resources and less time is spent spinning up a session for each call. Additionally, the model is only loaded once in the compute node and can be scored multiple times.

For realtime services, you do **not** need to specify:
+ inputs and outputs (dataframes are assumed)
+ code (only serialized models are supported)

Realtime web services do not support arbitrary code and only accept models created with the supported functions from packages installed with the product. See the following sections for a list of supported functions by language and package.

**See a realtime web service deployment example: [R](../operationalize/how-to-deploy-web-service-publish-manage-in-r.md#realtime-example)  |  [Python](../operationalize/python/how-to-deploy-manage-web-services.md#realtime-example)**

<a name=r></a>

### Supported functions in R

A model object created with these supported functions:

|R package|Supported functions|
|-------------|--------------------|
|[RevoScaleR](../r-reference/revoscaler/revoscaler.md)|rxLogit, rxLinMod, rxBTrees, rxDTree, and rxDForest| 
|[MicrosoftML](../r-reference/microsoftml/microsoftml-package.md)|Machine learning and transform tasks:<br/>rxFastTrees, rxFastForest, rxLogisticRegression, rxOneClassSvm, rxNeuralNet, rxFastLinear, featurizeText, concat, categorical, categoricalHash, selectFeatures, featurizeImage, getSentiment, loadimage, resizeImage, extractPixels, selectColumns, and dropColumns<br><br>While mlTransform featurization is supported in realtime scoring, R transforms are not supported. Instead, use sp_execute_external_script .|
<a name="inputdf"></a>
There are additional restrictions on the input dataframe format for microsoftml models:

1. The dataframe must have the same number of columns as the formula specified for the model.

1. The dataframe must be in the exact same order as the formula specified for the model.
  
1. The columns must be of the same data type as the training data. Type casting is not possible.

<a name=python></a>

### Supported functions in Python

|Python package|Supported functions|
|-------------|--------------------|
|[revoscalepy](../python-reference/revoscalepy/revoscalepy-package.md)|rx_btrees, rx_dforest, rx_dtree, rx_logit, rx_lin_mod|
|[microsoftml](../python-reference/microsoftml/microsoftml-package.md)|Machine learning and transform tasks:<br/>categorical, categorical_hash, concat, extract_pixels, featurize_text, featurize_image, get_sentiment, rx_fast_trees, rx_fast_forest, rx_fast_linear, rx_logistic_regression, rx_neural_network, rx_oneclass_svm, load_image, resize_image, select_columns, and drop_columns.<br/><br/>See the [input dataframe format restrictions above](#inputdf).|

## See also

[Deploy and manage web services in R](../operationalize/how-to-deploy-web-service-publish-manage-in-r.md)

[Deploy and manage web services in Python](../operationalize/python/how-to-deploy-manage-web-services.md)