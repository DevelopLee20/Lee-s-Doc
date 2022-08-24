# Python Doc

## 함수에 데이터형 명시

### 데이터형 종류
```text
NoneType : None의 데이터형
bool : 부울대수
int : 정수
float : 실수
complex : 복소수
str : 문자열
tuple : 튜플
list : 리스트
dict : 딕셔너리
function : 함수
```

### 예제 코드
```python
def test(a:int, b:float) -> str:
    return print("Hello World")
```

### 실행 결과
```text
test: (a: int, b: int) -> str
```

---

## 함수에 함수 설명 작성

### JSDoc 블록 태그 종류

[JSDoc Docs Link](https://jsdoc.app) in Block Tags

### 예제 코드
```python
def test(a:int, b:float) -> str:
    '''Hello World를 출력합니다.
    * @param {int} x - The x value.
    * @param {float} y - The y value.
    * @return {str} The return value.
    '''
    return print("Hello World")
```

### 실행 결과
```text
test: (a: int, b: float) -> str
Hello World를 출력합니다.

@param {int} x - The x value.
@param {float} y - The y value.
@return {str} The return value.
```

---
