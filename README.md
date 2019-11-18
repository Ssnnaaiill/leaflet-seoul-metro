# Leaflet.js를 활용하여 서울 지하철 지도 만들기

![](https://i.imgur.com/jvkZZ8I.png)

![](https://i.imgur.com/TKspOpV.mp4)

## 구현 목표

Leaflet에서 제공하는 API를 활용하여 서울 지하철 정보를 보여주는 지도 앱 구현을 목표로 하였습니다.

## 구현 방법

L.map의 setView 메소드를 사용하여 페이지가 처음 load되었을 때 default로 서울특별시가 보이도록 설정해 주었습니다. [서울의 각 지하철 역에 대한 세부 정보가 담긴 엑셀 파일](https://docs.google.com/spreadsheets/d/1BKHmNppyAgIuGfeFOoYA8807wNXsrIjnr3NcJPjPp4Y/edit?usp=sharing)을 JSON으로 변환하여 데이터를 불러오도록 해주었습니다.

각 지하철 역의 위도, 경도 데이터를 이용해 위치를 표시하는 마커는 custom icon 설정을 위해 circleMarker가 아닌 Marker를 채택하였고, flaticon에서 제공하는 아이콘으로 custom icon 설정을 해주었습니다. 이때 노선별로 다른 색의 마커를 표시해주어 노선별로 지하철 역을 구분할 수 있게 해주었습니다.
각 마커에 마우스 커서를 올렸을 때 해당 역의 이름, 노선, 도로명주소가 있는 tooltip이 나타나도록 해주었습니다.

최상단에는 z-index 설정으로 검색 바가 지도 위에 부양할 수 있도록 해주었습니다. 검색 바는 vuetify의 `v-autocomplete` 태그를 사용하여 입력한 문자열이 포함되어 있는 지하철 역명 또는 노선을 검색할 수 있습니다. 서로 다른 노선에 같은 이름을 가진 역(환승역)의 경우가 있기 때문에 지하철 역명과 노선 모두를 표기해 주었습니다.

`(+) 2019.11.18 추가`

Vuex를 사용하여 검색 바에서 입력한 역의 위치로 map의 중심을 이동시킬 수 있게 해주었습니다. 다른 곳으로 이동한다는 시각적인 효과를 주고 싶었고, 이를 위해서 zoom값을 지정해주는 것이 좋을 것 같아 flyTo 메소드를 채택하였습니다. 또한 Leaflet의 Marker clustering plugin을 사용하여 각 지하철 역의 마커들을 하나의 Marker group에 넣어 가까운 지역에 위치한 지하철 역들의 숫자를 클러스터로 표현할 수 있도록 구현하였습니다.

Vue Router를 사용하여 지도 앱을 실제로 보여지는 영역과 내부 구성 요소를 view와 components로 분리한 SPA로 만들었습니다.

## 어려웠던 점

local에 저장해둔 marker icon 파일이 불러와지지 않아 갈팡질팡한 경험이 있고, 지하철 역 데이터 엑셀 파일을 xlsx 라이브러리를 사용해 JSON 파일로 파싱하는 데에는 성공했으나 `key:value` 형식으로 만드는 데에 실패하여 결국 자동 툴을 사용하여 파싱하였습니다.

처음에는 v-autocomplete의 search-input.sync 속성을 사용하여 입력한 값을 불러오려 했는데, 페이지가 로드되고 나서 가장 처음에 역 이름을 입력하였을 때 null값이 불러와지는 문제가 있었습니다. 이는 Vuex의 store에 선언한 변수를 사용하면서 해결되었습니다.

## 추가 구현 희망

시간상의 문제로 입력한 지하철 역으로 map이 이동하는 기능을 구현하지 못하고 검색 바 UI만 구현해 두었는데, map 이동 기능도 추가로 구현해보고 싶습니다. 또한 각 노선의 역들을 잇는 선이 표시되도록 해보고 싶습니다. Leaflet의 Marker clustering plugin에 대하여 자세히 알아보고, 마커 클러스터링 기능도 언젠가 만들어보고 싶습니다.

`(+) 2019.11.18 추가`

Marker clustering을 구현할 때 어느 정도 group이 형성되기는 하나, map을 확대시키지 않은 상태에서도 마커들이 사라지지 않는 문제점이 있어 이를 해결하고 싶습니다.

---

Marker icon made by [Freepik](https://www.flaticon.com/authors/freepik)
