# ACCS 
Abnormal Climate Control System(이상기후통합관제체계)

# Frontend
## 사용기술 
 - React 
 - deck.gl 
 - chart.js 
 - Firestore: 게시판 데이터 CRUD를 프론트단에서 간결하게 처리 
 - firebase hosting: 웹 프론트앤드 배포

## 실행방법 
- 배포된 서비스 이용 (아래 url로 접근 가능)
> codingpotato-6daf2.web.app
- Local환경에서 실행 (React)
```
cd frontend
npm start
```
## 기능 설명
### 전국 무더위쉼터 지도 
- 지도에 무더위 쉼터의 위치를 표현
  (tooltip을 통해 근처 쉼터 갯수, 좌표 정보 제공)
- 각 지역의 하루 최대기온, 평균기온, 최소기온, 위험도를 표현
### 온열질환자 데이터
- 각 년도 기준 전체환자, 실외환자, 실내환자 수를 제공
- 각 년도 지역별 환자의 분포를 차트를 통해 표현
### 지원요청 게시판 
- 폭염 위험에 대처하기 위한 지원요청 게시판
- Firestore와 연동해 CRUD 구현 

# Backend
## 사용기술
  - Django
  - WebSocket: 실시간으로 client에게 폭염관련정보를 보내기 위함
  - BeautifulSoup
  - channels
  - docker: redis를 구동하기 위한 container
  - redis: channels를 구현하기 위해 서버
  
## 배포된 서비스 이용(아래 url로 접근 가능)
> http://34.64.174.66:8000/
 ```
 cd backend
 python manage.py runserver
 ```
 
 ## 기능 설명
 /heatwave/total
 > 온열질환자에 대한 전국 통계 api
 
 /heatwave/region
 > 온열질환자에 대한 지역별 통계 api
 
 /heatwave/response/<field>
 > 온열 대처요령 category별 api
 
 /shelter
 > 전국 무더위쉼터 정보 및 위치
 
 /ShareMe
 > 사용자 위치별 가장 가까운 무더위쉼터 정보, category별 대처요령 유저에게 socket을 통해 전송
 
 /Alarm
 > client와 socket 연결 및 socket을 통해 전달받은 정보 표시 
 
 ### Project Structure
 
 ```
Frontend
│  App.js
│  index.js
│
├─component
│  ├─BoardCard
│  │      index.js
│  │
│  ├─DeckMap
│  │      index.js
│  │
│  └─TempTable
│          index.js
│          styles.js
│
├─layouts
│  └─Appbar
│          index.js
│          styles.js
│
├─pages
│  ├─HSTable
│  │      index.js
│  │      styles.js
│  │
│  ├─ReqBoard
│  │  │  index.js
│  │  │  styles.js
│  │  │
│  │  └─ReqWrite
│  │          index.js
│  │          styles.js
│  │
│  └─TempMap
│          index.js
│          styles.js
│
└─routeAPI
        index.js
 ```
 
 ```
 Backend
│  .gitignore
│  manage.py
│
├─api_json
│      all_categories_heatwave_response.json
│      region_heat_wave.json
│      shelter.json
│      test_warning_data.json
│      total_heat_wave.json
│
├─api_module
│      areaCode.py
│      get_heatwave_warning.py
│      get_regions_temperature_info.py
│      heatwave_casualties_region.py
│      inquiry_response_heatwave.py
│      shelter.py
│
├─backend
│      asgi.py
│      routing.py
│      settings.py
│      urls.py
│      views.py
│      wsgi.py
│      __init__.py
│
├─user
│  │  finduser.py
│  │  user.py
│  │  views.py
│  │  __init__.py
│  │
│  └─migrations
│          __init__.py
│
└─websocket
    │  admin.py
    │  apps.py
    │  consumers.py
    │  models.py
    │  routing.py
    │  tests.py
    │  views.py
    │  __init__.py
    │
    ├─migrations
    │      0001_initial.py
    │      __init__.py
    │
    └─templates
            alarm.html
            ShareMe.html
 ```
