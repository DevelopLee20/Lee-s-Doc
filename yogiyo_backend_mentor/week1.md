# 요기요 멘토링 내용 정리

## 개발 방식

### 외부 API Client

- 외부 API를 불러와야할 경우 Client를 만들어, 따로 캡슐화해 관리하는 것이 좋다.

## SQL 관련

> 2024/09 기준 Postgre는 대세가 아니다.

## 리눅스 명령어 관련

### python3 실행파일 위치 찾는 방법

```bash
which python3
```

## Github 관련

### Github action

- 조건부(push 시 등등)에 스크립트를 작동하도록 할 수 있다.

## logging 관련

> logging은 print보다 오버헤드가 적다. 그 이유는 logging은 file에 write 하고, print는 terminal에 write 하기 때문이다. 또한 terminal에 write 하는 작업이 시간도 오래 걸린다.

## PyCharm 관련

### 맥 기준 단축키 모음

```text
command + 7     # 구조 확인
```

## pre-commit hook 관련

### Black

- 파일 포메터, 협업시 코드 스타일을 하나로 만들어준다.
- PEP8을 준수함
- Python Enhancement Proposals란? 파이썬으로 논쟁하는 커뮤니티, 의견이 맞으면 파이썬의 다음 버전으로 업데이트 됨

### mypy

- 코드의 타입 힌팅(hint)을 관리한다.
- strict = True 옵션이란? list 내부 value도 비교한다.
- 정적으로 문제를 찾아 해결할 수 있다.

## Python 코드 관련

### 튜플 Tuple

```python
tup1 = tuple(10)
tup2 = (10, )
print(tup1 == tup2) # True
```

- 튜플은 ()로 선언할 때 , 를 사용해야 한다. 이유는 그냥 괄호랑 헷갈리기 때문이다.

### 타입 힌팅 Type hinting ... 사용법

```python
def function(numbers: list[str, ...]) -> list[str, ...]:
    return numbers
```

- ...를 활용해 입력되는 list와 출력되는 list의 요소 제한을 없앴다.

### collection 이란?

- list, tuple, set 을 의미한다.
