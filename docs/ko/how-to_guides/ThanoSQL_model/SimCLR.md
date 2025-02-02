---
title: SimCLR
---

# __SimCLR__

__표기법 규칙__

- 괄호 `()`는 ^^리터럴^^ 괄호를 나타냅니다.
- 중괄호 {}는 옵션 조합을 묶는 데 사용됩니다.
- 대괄호 `[]`는 선택적 절을 나타냅니다.
- 대괄호 [ , ... ] 안에 있는 쉼표 다음에 오는 줄임표는 앞의 항목이 쉼표로 구분된 
목록으로 반복될 수 있음을 의미합니다.
- 세로 막대 `|`는 논리 `OR`를 나타냅니다.
- VALUE는 값을 의미합니다.

!!! note ""
    - __리터럴__: 고정되거나 변경할 수 없는 값을 의미하며 상수(Constant)라고도 불립니다.
    > 각 리터럴은 테이블에서 컬럼과 같은 특별한 자료형을 가지고 있습니다.

## __BUILD MODEL 구문__

"__BUILD MODEL__" 구문을 사용하여 인공지능 모델을 개발할 수 있습니다. "__BUILD MODEL__" 구문은 "__AS__" 뒤에 나오는 query_expr을 통해 정의된 데이터 세트를 사용하여 모델을 학습할 수 있습니다.
​
```sql
query_statement:
    query_expr

BUILD MODEL (model_name_expression)
USING SimCLR
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```
​
__OPTIONS 절__
​
```sql
OPTIONS (
    (image_col=VALUE),
    [batch_size=VALUE],
    [max_epochs=VALUE],
    [pretrained={True|False}],
    [overwrite={True|False}]
    )
```
​
"__OPTIONS__" 절에서 매개변수의 값을 기본값으로부터 변경할 수 있습니다. 각 매개변수의 의미는 아래와 같습니다.

- "image_col": 데이터 테이블에서 학습의 대상이 될 이미지의 경로를 담은 컬럼의 이름입니다. (str, default: 'image_path')
- "batch_size": 한 번의 학습에서 읽는 데이터 세트 묶음의 크기입니다. (int, optional, default: 256)
- "max_epochs": 모든 학습 데이터 세트를 학습하는 횟수를 설정합니다. (int, optional, default: 5)
- "pretrained": 사전 학습된 ImageNet 가중치 사용 여부를 설정합니다. (bool, optional, True|False, default: False)
- "overwrite": 동일 이름의 모델이 존재하는 경우 덮어쓰기 가능 여부를 설정합니다. True일 경우 기존 모델은 새로운 모델로 변경됩니다. (bool, optional, True|False, default: False)

__BUILD MODEL 예시__

[이미지로 이미지 검색하기](../../../tutorials/thanosql_search/search_image_by_image/)에서 "__BUILD MODEL__" 구문 사용 예시를 확인하실 수 있습니다.
​
```sql
%%thanosql
BUILD MODEL my_image_search_model
USING SimCLR
OPTIONS (
    image_col='image_path',
    max_epochs=1,
    overwrite=True
    )
AS
SELECT *
FROM mnist_train
```

## __CONVERT 구문__

"__CONVERT__" 구문은 데이터를 수치화한 벡터로 변환하고 이를 사용할 데이터 테이블에 추가합니다.
​
```sql
query_statement:
    query_expr

CONVERT USING (model_name_expression)
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```

__OPTIONS 절__

```sql
OPTIONS (
    (image_col=column_name),
    [result_col=column_name],
    [batch_size=VALUE]
    )
```
"__OPTIONS__" 절에서 매개변수의 값을 기본값으로부터 변경할 수 있습니다. 각 매개변수의 의미는 아래와 같습니다.

- "image_col": 데이터 테이블에서 이미지의 경로를 담은 컬럼의 이름입니다. (str, default: 'image_path')
- "result_col": 데이터 테이블에서 수치화 결과를 담을 컬럼 이름을 설정합니다. (str, optional, default: 'convert_result')
- "batch_size": 한 번의 학습에서 읽는 데이터 세트 묶음의 크기입니다. (int, optional, default: 256)

__CONVERT 예시__

[이미지로 이미지 검색하기](../../../tutorials/thanosql_search/search_image_by_image/)에서 "__CONVERT__" 구문 사용 예시를 확인하실 수 있습니다.
​
```sql
%%thanosql
CONVERT USING my_image_search_model
OPTIONS (    
    image_col='image_path',
    result_col='convert_result'
    )
AS
SELECT *
FROM mnist_test
```

## __SEARCH IMAGE 구문__

"__SEARCH IMAGE__" 구문을 사용하여 원하는 이미지를 검색할 수 있습니다.

```sql
query_statement:
    query_expr

SEARCH IMAGE
USING (model_name_expression)
OPTIONS (
    expression [ , ...]
    )
AS
(query_expr)
```

__OPTIONS 절__

```sql
OPTIONS (
    (search_by={image|text|audio|video}),
    (search_input=expression),
    (emb_col=column_name),
    [result_col=column_name],
    [top_k=VALUE]
    )
```

"__OPTIONS__" 절에서 매개변수의 값을 기본값으로부터 변경할 수 있습니다. 각 매개변수의 의미는 아래와 같습니다.

- "search_by": 검색할 때 사용할 이미지|텍스트|오디오|비디오 타입을 설정합니다. (str)
- "search_input": 검색할 때 사용할 입력값입니다. (str)
- "emb_col": 데이터 테이블에서 수치화된 결과를 담은 컬럼의 이름입니다. (str)
- "result_col": 데이터 테이블에서 검색 결과를 담을 컬럼 이름을 설정합니다. (str, optional, default: 'search_result')
- "top_k": 반환할 행의 수를 설정합니다. None을 입력할 시 데이터 테이블 전체를 반환합니다. (int, optional, default: 1000)

__SEARCH IMAGE 예시__

[이미지로 이미지 검색하기](../../../tutorials/thanosql_search/search_image_by_image/)에서 "__SEARCH IMAGE__" 구문 사용 예시를 확인하실 수 있습니다.

```sql
%%thanosql
SEARCH IMAGE
USING my_image_search_model
OPTIONS (
    search_by='image',
    search_input='thanosql-dataset/mnist_data/test/923.jpg',
    emb_col='convert_result',
    result_col='search_result'
    )
AS
SELECT *
FROM mnist_test
```