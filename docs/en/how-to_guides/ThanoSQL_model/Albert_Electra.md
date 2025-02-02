---
title: Albert & Electra
---

# __Albert & Electra__

__Notation Conventions__

- Parentheses `()` indicate ^^literal^^ parentheses.
- Braces `{}` are used to bind combinations of options.
- The bracket `[]` indicates an optional clause.
- An ellipsis following a comma in brackets [,...] means that the preceding item can be repeated as a comma-separated list.
- The vertical bar `|` represents the logic `OR`.
- VALUE represents a regular value.

!!! note ""
    - __literal__: a fixed or unchangeable value, also known as a Constant.
    > Each literal has a special data type such as column, in the table.

## __BUILD MODEL Syntax__

Use the "__BUILD MODEL__" statement to develop an AI model. The "__BUILD MODEL__" statement allows you to train a model using datasets defined with the query_expr that comes after the "__AS__" clause.

```sql
query_statement:
    query_expr

BUILD MODEL (model_name_expression)
USING { AlbertKo | AlbertEn | ElectraKo | ElectraEn }
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```

__OPTIONS Clause__

```sql
OPTIONS (
    (text_col=column_name),
    (label_col=column_name),
    [batch_size=VALUE],
    [max_epochs=VALUE],
    [learning_rate=VALUE],
    [overwrite={True|False}]
    )
```

The "__OPTIONS__" clause allows you to change the value of a parameter. The definition of each parameter is as follows.

- "text_col": the name of the column containing the text to be used for the training (str, default: 'text')
- "label_col": the name of the column containing information about the target (str, default: 'label')
- "batch_size": the size of dataset bundle utilized in a single cycle of training (int, optional, default: 16)
- "max_epochs": number of times to train with the training dataset (int, optional, default: 3)
- "learning_rate": the learning rate of the model (float, optional, default: 1e-4) 
- "overwrite": determines whether to overwrite a model if it already exists. If set as True, the old model is replaced with the new model (bool, optional, True|False, default: False)


__BUILD MODEL Example__

An example "__BUILD MODEL__" query can be found in [Create a Text Classification Model](../../../tutorials/thanosql_ml/classification/text_classification/).

```sql
%%thanosql
BUILD MODEL my_movie_review_classifier
USING ElectraEn
OPTIONS (
  text_col='review',
  label_col='sentiment',
  epochs=1,
  batch_size=4,
  overwrite=True
  )
AS
SELECT *
FROM movie_review_train
```


## __FIT MODEL Syntax__

Use the "__FIT MODEL__" statement to retrain an AI models. The "__FIT MODEL__" statement allows you to retrain a model using datasets defined with the query_expr that comes after the "__AS__" clause. In this case, the label of the data used for retraining should be the same as the label used for the previous training.

```sql
query_statement:
    query_expr

FIT MODEL (model_name_expression)
USING (model_name_expression)
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```

__OPTIONS Clause__

```sql
OPTIONS (
    (text_col=column_name),
    (label_col=column_name),
    [batch_size=VALUE],
    [max_epochs=VALUE],
    [learning_rate=VALUE],
    [overwrite={True|False}]
    )
```

The "__OPTIONS__" clause allows you to change the value of a parameter. The definition of each parameter is as follows.

- "text_col": the name of the column containing the text to be used for the training (str, default: 'text')
- "label_col": the name of the column containing information about the target (str, default: 'label')
- "batch_size": the size of dataset bundle utilized in a single cycle of training (int, optional, default: 16)
- "max_epochs": number of times to train with the training dataset (int, optional, default: 3)
- "learning_rate": the learning rate of the model (float, optional, default: 1e-4) 
- "overwrite": determines whether to overwrite a model if it already exists. If set as True, the old model is replaced with the new model (bool, optional, True|False, default: False)


## __PREDICT Syntax__

Use the "__PREDICT__" statement to apply AI models to perform prediction, classification, recommendation, and more. The "__PREDICT__" statement can preprocess the dataset defined by the query_expr that comes after the "__AS__" clause.

```sql
query_statement:
    query_expr

PREDICT USING (model_name_expression)
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```

__OPTIONS Clause__

```sql
OPTIONS (
    (text_col=column_name),
    [batch_size=VALUE],
    [result_col=column_name]
    )
```

The "__OPTIONS__" clause allows you to change the value of a parameter. The definition of each parameter is as follows.

- "text_col": the column containing the text to be used for prediction (str, default: 'text')
- "batch_size": the size of dataset bundle utilized in a single cycle of prediction (int, optional, default: 16)
- "result_col": the column that contains the predicted results (str, optional, default: 'predict_result')


__PREDICT Example__

An example "__PREDICT__" query can be found in [Create a Text Classification Model](../../../tutorials/thanosql_ml/classification/text_classification/).

```sql
%%thanosql
PREDICT USING my_movie_review_classifier
OPTIONS (
    text_col='review'
    )
AS
SELECT *
FROM movie_review_test
```

## __EVALUATE Syntax__

Use the "__EVALUATE__" statement to evaluate the AI model. The "__EVALUATE__" statement evaluates a model using the dataset defined by the query_expr that comes after the "__AS__" clause.

```sql
query_statement:
    query_expr

EVALUATE USING (model_name_expression)
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```

__OPTIONS Clause__

```sql
OPTIONS (
    (text_col=column_name),
    (label_col=column_name),
    [batch_size=VALUE]
    )
```

The "__OPTIONS__" clause allows you to change the value of a parameter. The definition of each parameter is as follows.

- "text_col": the column containing the text to be used for evaluation (str, default: 'text')
- "label_col": the name of the column containing information about the target (str, default: 'label')
- "batch_size": the size of dataset bundle utilized in a single cycle of evaluation (int, optional, default: 16)