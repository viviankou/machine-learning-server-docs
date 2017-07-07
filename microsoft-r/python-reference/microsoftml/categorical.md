--- 
 
# required metadata 
title: "Machine Learning Categorical Data Transform" 
description: "Categorical transform that can be performed on data before" 
keywords: "transform, category" 
author: "HeidiSteen" 
manager: "" 
ms.date: "" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

## ``categorical``: Convert text column into categories


*Applies to:* SQL Server 2017, Machine Learning Services 9.3


### Usage



```
microsoftml.categorical(cols: [<class ‘str’>, <class ‘dict’>, <class ‘list’>], output_kind: [‘Bag’, ‘Ind’, ‘Key’, ‘Bin’] = ‘Ind’, max_num_terms: int = 1000000, terms: int = None, sort: [‘Occurrence’, ‘Value’] = ‘Occurrence’, text_key_values: bool = False, **kargs)
```




### Description

Categorical transform that can be performed on data before
training a model.


### Details

The ``categorical`` transform passes through a data set, operating
on text columns, to build a dictionary of categories. For each row,
the entire text string appearing in the input column is defined as a
category. The output of the categorical transform is an indicator vector.
Each slot in this vector corresponds to a category in the dictionary, so
its length is the size of the built dictionary. The categorical transform
can be applied to one or more columns, in which case it builds a separate
dictionary for each column that it is applied to.

``categorical`` is not currently supported to handle factor data.


### Arguments


##### cols

A character string or list of variable names to transform. If
``dict``, the keys represent the names of new variables to be created.


##### output_kind

A character string that specifies the kind of output kind.

* ``"Bag"``: Outputs a multi-set vector. If the input column is a vector of categories, the output contains one vector, where the value in each slot is the number of occurrences of the category in the input vector. If the input column contains a single category, the indicator vector and the bag vector are equivalent 

* ``"Ind"``: Outputs an indicator vector. The input column is a vector of categories, and the output contains one indicator vector per slot in the input column. 

* ``"Key"``: Outputs an index. The output is an integer id (between 1 and the number of categories in the dictionary) of the category. 

* ``"Bin"``: Outputs a vector which is the binary representation of the category. 

The default value is ``"Ind"``.


##### max_num_terms

An integer that specifies the maximum number of
categories to include in the dictionary. The default value is 1000000.


##### terms

Optional character vector of terms or categories.


##### sort

A character string that specifies the sorting criteria.

* ``"Occurrence"``: Sort categories by occurences. Most frequent is first. 

* ``"Value"``: Sort categories by values. 


##### text_key_values

Whether key value metadata should be text, regardless of the actual input type.


##### kargs

Additional arguments sent to compute engine.


### Returns

An object defining the transform.


### See also

[``categorical_hash``](categorical_hash.md)


### Example



```
'''
Example on rx_logistic_regression and categorical.
'''
import numpy
import pandas
from microsoftml import rx_logistic_regression, categorical, rx_predict

train_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Do not like it", "Really like it",
        "I hate it", "I like it a lot", "I kind of hate it", "I do like it",
        "I really hate it", "It is very good", "I hate it a bunch", "I love it a bunch",
        "I hate it", "I like it very much", "I hate it very much.",
        "I really do love it", "I really do hate it", "Love it!", "Hate it!",
        "I love it", "I hate it", "I love it", "I hate it", "I love it"],
    like=[True, False, True, False, True, False, True, False, True, False,
        True, False, True, False, True, False, True, False, True, False, True,
        False, True, False, True]))
        
test_reviews = pandas.DataFrame(data=dict(
    review=[
        "This is great", "I hate it", "Love it", "Really like it", "I hate it",
        "I like it a lot", "I love it", "I do like it", "I really hate it", "I love it"]))

# Use a categorical transform: the entire string is treated as a category
out_model = rx_logistic_regression("like ~ reviewCat",
                data=train_reviews,
                ml_transforms=[categorical(cols=dict(reviewCat="review"))])
                
# Note that 'I hate it' and 'I love it' (the only strings appearing more than once)
# have non-zero weights.
print(out_model.coef_)

# Use the model to score.
source_out_df = rx_predict(out_model, data=test_reviews, extra_vars_to_write=["review"])
print(source_out_df.head())
```


Output:



```
Beginning processing data.
Rows Read: 25, Read Time: 0, Transform Time: 0
Beginning processing data.
Not adding a normalizer.
Warning: The number of threads specified in trainer arguments is larger than the concurrency factor setting of the environment. Using 2 training threads instead.
LBFGS multi-threading will attempt to load dataset into memory. In case of out-of-memory issues, turn off multi-threading by setting trainThreads to 1.
Beginning processing data.
Rows Read: 25, Read Time: 0, Transform Time: 0
Beginning processing data.
Beginning optimization
num vars: 20
improvement criterion: Mean Improvement
Warning: Premature convergence occurred. The OptimizationTolerance may be set too small. ro equals zero. Is your function linear?
L1 regularization selected 3 of 20 weights.
Not training a calibrator because it is not needed.
Elapsed time: 00:00:00.9857015
Elapsed time: 00:00:00.2370012
OrderedDict([('(Bias)', 0.2132386565208435), ('I hate it', -0.7938994765281677), ('I love it', 0.19674676656723022)])
Beginning processing data.
Rows Read: 10, Read Time: 0, Transform Time: 0
Beginning processing data.
Elapsed time: 00:00:00.1974495
Finished writing 10 rows.
Writing completed.
           review PredictedLabel     Score  Probability
0   This is great           True  0.213239     0.553109
1       I hate it          False -0.580661     0.358781
2         Love it           True  0.213239     0.553109
3  Really like it           True  0.213239     0.553109
4       I hate it          False -0.580661     0.358781
```
