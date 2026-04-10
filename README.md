<div align="center">

# 먹어볼래? (Mukablorae)

### AI 기반 블로그 광고 판별 & 리뷰 요약 맛집 탐색 서비스

> 광고성 블로그 리뷰를 판별하고, 실제 방문자 리뷰를 요약·해시태그화하여  
> 신뢰도 높은 맛집 탐색을 돕는 AI 기반 웹 서비스

<br/>

<img src="https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
<img src="https://img.shields.io/badge/React-18-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" />
<img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/Transformers-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black" />

</div>

---

## 목차

- [프로젝트 소개](#프로젝트-소개)
- [발표 및 수상](#발표-및-수상)
- [문제 정의](#문제-정의)
- [주요 기능](#주요-기능)
- [기술 스택](#기술-스택)
- [시스템 아키텍처](#시스템-아키텍처)
- [시스템 흐름](#시스템-흐름)
- [프로젝트 구조](#프로젝트-구조)
- [설계 및 구현 포인트](#설계-및-구현-포인트)
- [성능 평가](#성능-평가)
- [설치 및 실행](#설치-및-실행)
- [모델 파일 안내](#모델-파일-안내)
- [API 엔드포인트](#api-엔드포인트)
- [트러블슈팅](#트러블슈팅)
- [한계 및 개선 방향](#한계-및-개선-방향)
---

## 프로젝트 소개

대구대학교 하양캠퍼스 주변 맛집의 블로그 리뷰를 분석하여  
**광고성 글과 실제 후기를 구별**하고, **리뷰 요약**과 **감성 키워드 분석**을 통해  
더 신뢰도 높은 맛집 탐색을 돕는 서비스입니다.

기존 맛집 탐색 과정에서는 광고성 블로그 글이 많아 정보 신뢰도가 낮고,  
실제 방문자 리뷰도 양이 많아 핵심 내용을 빠르게 파악하기 어렵다는 문제가 있습니다.

**먹어볼래?**는 이러한 문제를 해결하기 위해 다음 기능을 하나의 흐름으로 통합했습니다.

- 광고성 블로그 판별
- 리뷰 자동 요약
- 감성 키워드 및 점수 분석
- 검색 및 상세페이지 기반 맛집 정보 제공

### 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **작품명** | 블로그 광고 필터링 및 리뷰 요약 서비스 |
| **프로젝트 형태** | AI + 웹 서비스 통합 프로젝트 |
| **개발 인원** | 3명 |
| **팀명** | 와구와구 |
| **제출일** | 2025.06.04 |

---

## 발표 및 수상

### 발표 영상
🎥 [졸업작품 발표 영상 보기](https://www.youtube.com/watch?v=kHd3aq7gIvA)

### 수상 내역
- **2025학년도 IT·공학대전 학생작품 경진대회 장려상**
- **작품명**: AI기반 맛집 검색 시스템 - 먹어볼래
- **팀명**: 와구와구
- **팀원**: 이인호, 구승율, 김민규
- **수상일**: 2025.11.25
- **주최**: 대구대학교 IT·공과대학

<div align="center">
  <img src="https://github.com/user-attachments/assets/e866b609-3361-4ce8-a969-879d858c222c" alt="먹어볼래 수상 상장" width="420" />
</div>

---


## 담당 역할

- 서비스 전체 구조에 대한 개념설계 및 상세설계 수행
- FastAPI 기반 백엔드 API 구성 및 프론트엔드 연동
- 사용자 검색 → 블로그 수집 → 광고 판별 → 결과 반환 흐름 연결
- 음식점 상세 조회 API에서 리뷰 요약, 감성 점수, 해시태그, 광고 확률을 통합한 JSON 응답 구성
- 네이버 검색 API 및 블로그/리뷰 데이터 수집·전처리
- 감성사전 기반 해시태그 생성 및 후처리
- 모델 결과와 서비스 데이터를 연결하는 응답 구조 정리

---

## 문제 정의

맛집 탐색 과정에는 다음과 같은 어려움이 있습니다.

- 광고성 블로그 리뷰가 많아 식당 정보의 신뢰도가 낮음
- 방문자 리뷰 수가 많아 핵심 내용을 빠르게 확인하기 어려움
- 단순 평점만으로는 사용자의 방문 목적에 맞는 식당 선택이 어려움

이를 해결하기 위해 **광고성 콘텐츠 필터링**, **리뷰 요약 제공**,  
**감성 키워드 시각화 및 점수화**를 중심으로 서비스를 설계했습니다.

---

## 주요 기능

| 기능 | 설명 |
|------|------|
| **블로그 광고 판별** | KoBERT 기반 분류 모델 + 규칙 기반 하이브리드 방식으로 광고/비광고 여부 및 광고 확률 판별 |
| **리뷰 요약 결과 제공** | KoBART를 활용해 방문자 리뷰를 **긍정 / 부정 / 전체 요약** 형태로 제공 |
| **감성 분석 및 해시태그 추출** | Mecab, KiWiPiepy, 감성사전 기반 키워드 추출 및 감성 점수 산출 |
| **검색 및 상세페이지 제공** | 리뷰 요약, 감성 키워드, 광고 확률, 위치 정보 등을 한 화면에서 제공 |
| **실시간 블로그 크롤링** | 네이버 블로그 검색 API 및 크롤링 기반 리뷰 수집 |
| **OCR 분석** | 이미지 내 텍스트를 추출해 광고 문구 탐지 |
| **피드백 시스템** | 사용자 피드백 수집 및 통계 대시보드 제공 |

---

## 기술 스택

| 구분 | 기술 |
|------|------|
| **Frontend** | React 18, React Bootstrap, React Router, Chart.js |
| **Backend** | FastAPI, Python 3.9, Uvicorn |
| **AI / ML** | PyTorch, Transformers, KoBERT, KoBART |
| **NLP** | Mecab, KiWiPiepy, KNU Sentiment Lexicon, RapidFuzz, NLTK |
| **Crawling / OCR** | 네이버 검색 API, Selenium, BeautifulSoup4, aiohttp, Tesseract OCR |
| **Data Processing** | Pandas, JSON, CSV |

---

## 시스템 아키텍처

<div align="center">
  <img src="https://github.com/user-attachments/assets/c31efd62-a197-4dee-88c7-c1811da02063" alt="시스템 아키텍처" width="900" />
</div>

---

## 시스템 흐름

<div align="center">
  <img src="https://github.com/user-attachments/assets/36e330ae-8f11-4fdc-87fb-5c6b52e22fe5" alt="시스템 흐름" width="900" />
</div>

---

## 프로젝트 구조

```text
Mukablorae/
├── backend/                        # FastAPI 백엔드
│   ├── main.py                     # 메인 서버 (API 엔드포인트)
│   ├── data.py                     # 맛집 데이터베이스
│   ├── feedback_system.py          # 피드백 시스템
│   ├── blog_model/                 # KoBERT 광고 판별 모델
│   ├── summary_model/              # KoBART 리뷰 요약 모델
│   ├── processed_reviews.csv       # 리뷰 데이터셋
│   └── store_sentiment_result.csv  # 감성 분석 결과
│
├── frontend/                       # React 프론트엔드
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Home.js
│   │   │   ├── Results.js
│   │   │   ├── Information.js
│   │   │   └── FeedbackDashboard.js
│   │   ├── components/
│   │   └── contexts/
│   └── package.json
│
├── training/                       # ML 모델 학습 스크립트
│   └── kobart.ipynb
│
├── etc/                            # 유틸리티 스크립트
└── requirements.txt
```

---

## 설계 및 구현 포인트

### 1. FastAPI 기반 통합 백엔드 구성
광고 판별, 리뷰 요약, 감성 해시태그, 지도 정보를 각각 분리하지 않고  
FastAPI 서버에서 하나의 서비스 흐름으로 연결했습니다.

### 2. 검색 API와 상세 API 분리
검색 결과를 제공하는 API와 음식점 상세 정보를 반환하는 API를 나누어  
프론트엔드가 화면 단위로 데이터를 소비할 수 있도록 구성했습니다.

### 3. 응답 구조 단순화
상세페이지에서 필요한 음식점 정보, 감성 점수, 해시태그, 광고 확률,  
리뷰 요약 결과를 하나의 JSON 구조로 묶어 프론트엔드가 바로 사용할 수 있도록 설계했습니다.

### 4. AI 결과의 서비스화
KoBERT 광고 판별과 KoBART 리뷰 요약 결과를 모델 출력에 그치지 않고  
실제 화면에 연결되는 서비스 응답으로 통합했습니다.

---

## 성능 평가

### 광고 판별 모델 (KoBERT)

| 지표 | 값 |
|------|----|
| Accuracy | **0.81** |
| Recall | **0.77** |
| Precision | **0.78** |
| F1-score | **0.80** |

### 리뷰 요약 모델 (KoBART)

| 지표 | 값 |
|------|----|
| ROUGE-1 | **20.9%** |
| ROUGE-2 | **9.2%** |
| ROUGE-L | **19.5%** |

---

## 설치 및 실행

### 사전 요구사항

- Python 3.9+
- Node.js 14+
- Tesseract OCR
- 네이버 검색 API 키

### Backend 실행

```bash
pip install -r requirements.txt
```

`.env` 파일 생성:

```env
NAVER_CLIENT_ID=<your-client-id>
NAVER_CLIENT_SECRET=<your-client-secret>
```

서버 실행:

```bash
python backend/main.py
```

- Backend: `http://127.0.0.1:8013`

### Frontend 실행

```bash
cd frontend
npm install
npm start
```

- Frontend: `http://localhost:3000`

---

## 모델 파일 안내

모델 가중치 파일(`.pt`, `.safetensors`)은 용량 문제로 Git에 포함하지 않았습니다.  
별도로 다운로드한 뒤 아래 경로에 배치해야 합니다.

- `backend/blog_model/` → KoBERT 광고 판별 모델
- `backend/summary_model/` → KoBART 리뷰 요약 모델

---

## API 엔드포인트

| Method | Endpoint | 설명 |
|--------|----------|------|
| `GET` | `/health` | 서버 상태 확인 |
| `POST` | `/api/warmup` | AI 모델 사전 로딩 |
| `POST` | `/api/crawl_predict` | 블로그 크롤링 및 광고 판별 |
| `GET` | `/api/restaurant/{name}` | 맛집 상세 정보 조회 |
| `POST` | `/api/feedback/*` | 사용자 피드백 제출 |
| `GET` | `/api/feedback/stats` | 피드백 통계 조회 |

---

## 트러블슈팅

### 1. 실시간 처리 지연
네이버 API 기반 블로그 수집과 분석이 실시간으로 이뤄지다 보니  
응답 시간이 길어지는 문제가 있었습니다.

이를 완화하기 위해 수집과 분석 단계를 분리하고,  
반복 요청에 대해서는 캐시를 적용할 수 있도록 구조를 정리했습니다.

### 2. 응답 데이터 구조 복잡도
광고 판별, 리뷰 요약, 감성 해시태그, 지도 정보가 서로 다른 단계에서 생성되어  
프론트엔드에서 바로 사용하기 어려웠습니다.

이를 해결하기 위해 상세페이지 기준의 JSON 응답 구조로 재구성했습니다.

### 3. 리뷰 텍스트 후처리
리뷰 원문은 표현이 다양하고 그대로는 서비스 화면에 사용하기 어려웠습니다.

이를 해결하기 위해 형태소 분석과 감성사전 매칭을 통해  
해시태그와 점수로 가공하는 후처리 단계를 두었습니다.

---

## 한계 및 개선 방향

- 실시간 네이버 API 기반 처리 특성상 응답 시간이 길어질 수 있음
- 현재는 대구대학교 주변 맛집 중심으로 서비스 범위가 제한됨
- KoBART 리뷰 요약 품질은 추가 개선 여지가 있음

향후에는 다음 방향으로 개선할 계획입니다.

- API 구조 분리
- 응답 검증 모델 적용
- 캐시 및 저장 구조 고도화
- 리뷰 요약 성능 개선
- 서비스 대상 지역 확장

---



