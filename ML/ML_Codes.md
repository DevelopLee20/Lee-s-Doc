# ML 구현 코드 정리

## 데이터 불러오기

```python
import pandas as pd

data = pd.read_csv('{CSV_File_Name.csv}')
```
- DataFrame 데이터형을 가지고 있다.

---

## 데이터에서 필요없는 부분 제거

### 데이터 드롭(Drop)

```python
data = data.drop(['{Column_Name1}','Column_Name2'], axis=1)
```
- axis=1 옵션을 넣어주지 않을 경우 에러 발생

### 데이터 내의 결측값 제거

```python
data = data.dropna(axis=0) # Column을 제거
data = data.dropna(axis=1) # Row을 제거
```
- 데이터의 상황에 따라서 axis 옵션을 다르게 준다.

---

## 라벨(Label) 카테고리(Category) 데이터를 숫자로 변환

```python
category = dict((c, i) for i, c in enumerate(set(cateogory_y)))

y = [category[i] for i in cateogory_y]
```

- category 데이터형는 숫자형이 아닌 문자를 데이터로 가지고 있을때 사용된다.
- 이 데이터를 데이터로 사용시 숫자로 자동 변환을 해주지 않기 때문에 변경해주어야 한다.

---

## 분류 개수 확인

```python
Classfier = len(set(y))
```

---

## 데이터 전처리

### 라벨데이터 스케일러

```python
import sklearn.preprocessing as scalers

ScaleType = scalers.MinMaxScaler() # 최대최소 스케일러
ScaleType = scalers.StandardScaler() # 표준화 스케일러
X = X.to_numpy()
X_scale = ScaleType.fit_transform(X)
```

- 성공적인 전처리는 속도향상의 장점과 특이점 분류에 장점이 있다.
- 더욱 많은 스케일러가 있지만 난 주로 위의 스케일러를 사용한다.

### 타겟데이터 원-핫 인코딩

```python
import tensorflow.keras.utils as encoding

y = encoding.to_categorical(y, Classfier)
```
- 모델에 소프트 맥스(Softmax)가 사용될 때 사용한다.

---

## 데이터 분할

```python
from sklearn.model_selection import train_test_split

data_split = 0.2
X_train, X_val, y_train, y_val = train_test_split(X, y, stratify=y, test_size=data_split)
X_train, X_test, y_train, y_test = train_test_split(X_train, y_train, stratify=y_train, test_sizedata_split)
```

- 훈련 데이터와 테스트 데이터 이외의 검증 데이터(val_data)를 생성해 모델의 범용성을 높인다.

---

## 그리드 서치(Grid Search)용 모델 생성

### 모델 생성

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
from tensorflow.keras import optimizers

def model_build({option1, option2, option3, option4}):
    model = Sequential()
    model.add()
    model.compile()
    return model
```

- 원하는 구조의 모델을 생성한 후, 원하는 옵션을 매개변수에 추가한다.

### 생성한 모델을 분류기 객체에 저장

```python
from tensorflow.keras.wrappers.scikit_learn import KerasClassifier

model = KerasClassifier(build_fn=model_build, verbose=0)
```

- 일반 함수를 케라스 분류기 객체를 생성해서 넣어준다.
- 이 과정을 거쳐야 그리드 서치(Grid Search) 매개변수 지정이 가능해진다.

### 그리드 서치 매개변수 지정

```python
params_grid = {
    "option1" = [1,2,3],
    "option2" = [1,2,3],
    "option3" = [1,2,3],
    "option4" = [1,2,3],
}
```

- 튜플형으로 생성해야한다.

### 그리드 서치 실행

```python
from sklearn.model_selection import GridSearchCV

grid_search = GridSearchCV(
    estimator=model,
    param_grid=params_grid,
    verbose=1,
    n_jobs=16,
)

grid_results = grid_search.fit(X_train, y_train)
```

- verbose 옵션은 0: 실행 중간 결과 미출력, 1: 실행 중간 결과 출력, 2: 상세 중간 결과 출력한다.
- n_jobs 옵션은 학습에 사용할 CPU 코어 개수를 의미한다. -1을 넣으면 사용 가능한 만큼 사용한다.

### 최적의 모델의 매개변수 확인

```python
grid_results.best_params_
```

### 최적의 매개변수로 모델 생성

```python
best_model = model_build(option1, option2, option3, option4)
result = best_model.fit(X, y, epochs=option5)
```

### 그리드 서치 사용시 경고문 제거

```python
import warnings

warnings.filterwarnings(action='ignore')
warnings.filterwarnings(action='default')
```

- 파이썬에서 제공하는 경고문을 무시할 수 있다.

---

## 모델 저장 및 불러오기

### 생성한 모델 파일로 저장

```python
from tensorflow.keras.models import load_model

model_name = f'{file_name}.h5'

best_model.save(model_name)
```

- 모델의 확장자 명은 .h5 이다.

### 생성한 모델 파일 불러오기

```python
save_model = load_model(f'{model_name}.h5')
save_model.predict_classes(X_val)
```

- 모델이 출력하는 예측값이 나온다.

---

## 간단한 psql 형식표 출력하기

```python
from tabulate import tabulate

a = tabulate(
    tabular_data=data,
    headers=columns,
    tablefmt='psql'
)
```

---
