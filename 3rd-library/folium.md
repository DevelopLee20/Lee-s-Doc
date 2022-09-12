# folium

- 지도를 이용해 표시하는 라이브러리

## 라이브러리 설치
```python
!pip install folium
```

## 지도 객체 생성

```python
import folium

folium_map = folium.Map(location=[36, 127], zoom_state=1)
```

- location : 표시할 지도의 위도(float)와 경도(float)를 입력
- zoom_state : 표시할 지도의 확대 정도(int)를 설정

## 지도 출력

```python
folium_map # print(folium_map)
```

## 지도에 마커 추가

```python
folium.Marker(
    [37, 127],
    tooltip = 'Marker name'
    ).add_to(folium_map)
```
