---
title: PREDICT
---

# __PREDICT__

## __1. PREDICT Statement__

The "__PREDICT__" statement allows users to apply models to perform prediction, classification, recommendation, and more.


## __2. PREDICT Syntax__

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

## __3. PREDICT Example__

!!! note
    - Examples are specific to one model, and the required option values ​​or the dataset used may differ from model to model. For a detailed description of each model, refer to the [ThanoSQL Model Statement Reference](../../reference/#model-statement)
    - Because the example only works when a specific model and dataset are present, it may not run normally if copied and used as is.

```sql
%%thanosql
PREDICT USING my_product_classifier
OPTIONS (
    image_col='image_path',
    result_col='predict_result'
    )
AS
SELECT *
FROM product_image_test
```


!!! note "AI Models That Can Be Used with '__PREDICT__ Statement'"
    - AutoML Classification model - AutomlClassifier
    - AutoML Regression model - AutomlRegressor
    - ConvNeXT Model - ConvNeXt_Tiny, ConvNeXt_Base
    - EfficientNet Model - EfficientNetV2S, EfficientV2M
    - Albert Model - AlbertKo, AlbertEn
    - Electra Model - ElectraKo, ElectraEn
    - Wav2Vec2 Model - Wav2Vec2Ko, Wav2Vec2En
    - Whisper Model - Whisper
    - ViLT Model - ViLT
    - TFT Model - TFT