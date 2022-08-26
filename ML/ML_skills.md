# ML 잡기술 정리

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
- 곧 작성

## 그리드 서치(Grid Search)용 모델 생성
- 곧 작성

## 모델 저장 및 불러오기
- 곧 작성
