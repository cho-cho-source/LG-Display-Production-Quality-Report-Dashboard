
**날짜 : 2026년 09월 14일**

<img width="985" height="795" alt="image" src="https://github.com/user-attachments/assets/07059f29-3d22-45b6-8e71-76d70da49cfe" />





LG디스플레이 생산·품질 업무를 이해하기 위해 제작한 **교육용 생산·품질 보고서 웹 서비스**입니다.
Google Colab 환경에서 Flask 기반 웹 서버를 실행하고, Three.js를 이용하여 패널 검사·이송 설비를 3D로 구현했습니다.

생산 LOT 데이터를 조회하여 생산 실적, 목표 달성률, 검사 수량, 불량률을 확인할 수 있으며, 조회한 데이터를 근거로 생산·품질 보고서를 생성하고 관리할 수 있습니다.

---

## 사용 라이브러리 및 기술

### Backend

* Python
* Flask 3.1.2
* Werkzeug
* JSON
* Threading
* Asyncio

### Frontend

* HTML5
* CSS3
* JavaScript
* Three.js 0.170.0
* OrbitControls
* WebGL

### Development / Deployment

* Google Colab
* Cloudflared Quick Tunnel
* IPython Display

---

## 설명

### 1. 생산·품질 데이터 조회

사용자가 조회 기간과 생산라인을 선택하여 생산 및 품질 데이터를 확인할 수 있도록 구현했습니다.

조회 가능한 주요 데이터는 다음과 같습니다.

* 생산 목표 수량
* 실제 생산 수량
* 생산 목표 달성률
* 검사 수량
* 불량 수량
* 불량률
* LOT별 생산·검사 실적
* 기간별 설비 사건

생산 달성률은 다음과 같이 계산합니다.

`생산 달성률 = 생산 수량 합계 / 목표 수량 합계 × 100`

불량률은 다음과 같이 계산합니다.

`불량률 = 불량 수량 합계 / 검사 수량 합계 × 100`

검사 수량이 0개인 경우에는 불량률을 계산하지 않고 **판단 불가**로 표시합니다.

---

### 2. LOT 기반 생산 이력 관리

생산라인 A/B의 LOT 데이터를 기반으로 생산 및 품질 실적을 조회할 수 있습니다.

조회 조건에 따라 다음 정보가 함께 변경됩니다.

* LOT 정보
* 목표 수량
* 생산 수량
* 검사 수량
* 불량 수량
* 생산 달성률
* 불량률
* 일자별 불량률

이를 통해 특정 기간이나 생산라인의 생산·품질 상태를 한 화면에서 확인할 수 있도록 구성했습니다.

---

### 3. 설비 이벤트 조회

조회 기간에 발생한 생산 설비 이벤트를 생산 데이터와 함께 확인할 수 있도록 구현했습니다.

예시 이벤트는 다음과 같습니다.

* 설비 정지
* 온도 주의
* 정상 복귀
* 미해제 설비 이상

생산 실적과 설비 사건을 함께 확인함으로써 생산 및 품질 변화가 발생한 시점을 비교할 수 있도록 했습니다.

---

### 4. Three.js 기반 패널 검사 설비 구현

Three.js와 WebGL을 활용하여 실제 디스플레이 패널 검사·이송 설비를 참고한 3D 모의 설비를 구현했습니다.

주요 구성 요소는 다음과 같습니다.

* 금속 설비 프레임
* 롤러 컨베이어
* 디스플레이 패널
* 검사 헤드
* 검사 카메라
* 투명 안전 커버
* 작업자 조작 패널
* 설비 상태등
* 조명 및 그림자

패널은 롤러 위를 따라 이동하도록 구현했으며 사용자가 마우스를 이용하여 설비를 자유롭게 확인할 수 있습니다.

**조작 방법**

* 마우스 드래그 : 화면 회전
* 마우스 휠 : 확대 / 축소
* 시점 초기화 : 기본 카메라 위치 복귀

---

### 5. 설비 상태 시각화

생산라인에 따라 현재 모의 설비 상태를 다르게 표시합니다.

예시

* Line A : 정상 / 28°C
* Line B : 온도 주의 / 42°C

3D 설비의 Tower Lamp도 설비 상태와 연동하여 상태를 직관적으로 확인할 수 있도록 구성했습니다.

---

### 6. 생산·품질 보고서 자동 생성

현재 조회된 생산·품질 데이터를 근거로 보고서 초안을 생성할 수 있습니다.

보고서에는 다음 내용이 포함됩니다.

* 조회 기간
* 조회 생산라인
* 생산 수량
* 목표 수량
* 생산 달성률
* 검사 수량
* 불량 수량
* 불량률
* LOT 기록 수
* 조회 기간 내 설비 사건
* 현재 모의 설비 상태
* 추가 확인 사항

보고서 생성 시점의 데이터를 **생성 근거(Evidence)** 로 함께 저장하여 이후 조회 조건이 변경되더라도 기존 보고서의 근거 데이터가 변경되지 않도록 구성했습니다.

---

### 7. 보고서 편집 및 검토

생성된 보고서의 본문을 사용자가 직접 수정할 수 있습니다.

* 보고서 본문 : 1~6000자
* 보고서 수정
* 검토 완료 여부 설정
* 최종 수정 시간 저장
* 기존 생성 근거 유지

본문을 수정하면 기존 검토 완료 상태가 해제되어 수정된 보고서를 다시 확인하도록 구성했습니다.

---

### 8. 보고서 저장 및 다운로드

생성된 보고서는 JSON 파일을 이용하여 관리합니다.

보고서별로 다음 두 가지 파일을 다운로드할 수 있습니다.

**TXT**

* 최종 저장된 보고서 본문
* 보고서 번호
* 검토 상태
* 저장 시간

**JSON**

* 보고서 생성 당시 조회 조건
* LOT 데이터
* 생산·품질 집계 결과
* 설비 이벤트
* 현재 모의 설비 상태

이를 통해 보고서 결과뿐만 아니라 해당 보고서를 작성할 때 사용한 근거 데이터까지 확인할 수 있도록 했습니다.

---

### 9. Google Colab 실행

본 프로젝트는 별도의 로컬 개발환경 구축 없이 **Google Colab의 하나의 Python Cell에서 실행**할 수 있도록 제작했습니다.

실행 과정은 다음과 같습니다.

`Google Colab → Flask Server → HTML/CSS/JavaScript → Three.js → Cloudflared → External Browser`

Flask 서버가 사용 가능한 포트를 자동으로 탐색하여 실행되며, Cloudflared Quick Tunnel을 통해 외부 브라우저에서도 웹페이지에 접속할 수 있습니다.

Colab 런타임을 다시 실행하면 기존 서버 및 Cloudflared Tunnel을 종료한 후 새로운 서버를 생성하도록 구성했습니다.

---

## 프로젝트 구조

```text
04_lgd_web/
│
├── 04_dashboard.html
├── 04_reports.json
├── 04_cloudflared.log
├── cloudflared
│
└── static/
    └── vendor/
        ├── three.module.js
        └── OrbitControls.js
```

---

## 주요 기능 요약

| 기능        | 설명                          |
| --------- | --------------------------- |
| 생산 데이터 조회 | 날짜와 생산라인별 LOT 조회            |
| 목표 달성률    | 생산 실적 / 목표 수량 계산            |
| 품질 데이터 조회 | 검사 수량과 불량 수량 조회             |
| 불량률       | 불량 / 검사 수량 계산               |
| 설비 사건 관리  | 기간 내 설비 이상 이력 확인            |
| 3D 설비     | Three.js 기반 검사·이송 설비 구현     |
| 설비 상태 표시  | 정상 / 온도 주의 상태 시각화           |
| 보고서 생성    | 조회 데이터를 기반으로 초안 생성          |
| 보고서 편집    | 생성된 보고서 직접 수정               |
| 검토 상태 관리  | 보고서 검토 완료 여부 저장             |
| TXT 다운로드  | 최종 보고서 본문 다운로드              |
| JSON 다운로드 | 보고서 생성 근거 데이터 다운로드          |
| 외부 접속     | Cloudflared Quick Tunnel 사용 |

---

## 참고 문헌 및 자료

### Flask

* Flask Documentation
  https://flask.palletsprojects.com/

### Three.js

* Three.js Documentation
  https://threejs.org/docs/

* Three.js GitHub
  https://github.com/mrdoob/three.js

* Three.js CDN
  https://cdn.jsdelivr.net/npm/three@0.170.0/build/three.module.js

### OrbitControls

* Three.js OrbitControls
  https://threejs.org/docs/#examples/en/controls/OrbitControls

* OrbitControls Module
  https://cdn.jsdelivr.net/npm/three@0.170.0/examples/jsm/controls/OrbitControls.js

### Cloudflare Tunnel

* Cloudflare Tunnel Documentation
  https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/

* Cloudflared GitHub
  https://github.com/cloudflare/cloudflared

### Google Colab

* Google Colaboratory
  https://colab.research.google.com/

---

## 참고 사항

본 프로젝트의 생산·품질 데이터와 설비 상태는 **LG디스플레이 직무 및 생산·품질 업무 이해를 위한 교육용 가상 데이터**입니다.

실제 LG디스플레이 생산설비, 제조 데이터, 공정 데이터 또는 사내 시스템을 구현한 것이 아니며, 생산·품질 데이터 처리 및 업무 보고 과정을 학습하기 위한 목적으로 제작했습니다.

---

## Repository Topics

`lg-display` `manufacturing` `quality-control` `flask` `threejs` `webgl` `google-colab` `cloudflared` `dashboard` `production-dashboard`
