# haversine

- 위도와 경도를 다른 단위로 치환해주는 라이브러리

## 라이브러리 설치

```python
!pip install haversine
```

## 위도 경도 계산

```python
from haversine import haversine

start = (37,127)
end = (38,128)
haversine((start,end)) # km 단위로 출력
```

- haversine : 시작점(tuple)과 끝점(tuple)을 차례로 입력, km 단위로 두 점의 거리가 치환된다.
