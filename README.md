# 안녕하세요.

LLM 협업 방법론을 직접 설계하고,
AI를 활용해 실동작하는 서비스를 만드는 개발자입니다.


<!-- ![](https://komarev.com/ghpvc/?username=gmkoo-d3v&color=blue) -->
<!--
## 🛠 Tech Stack

| Category | Technologies |
|---|---|
| **Backend** | ![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white) ![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white) |
| **Frontend** | ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) ![Zustand](https://img.shields.io/badge/Zustand-20232a?style=flat-square&logo=react&logoColor=61DAFB) ![MUI](https://img.shields.io/badge/MUI-0081CB?style=flat-square&logo=mui&logoColor=white) ![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white) |
| **AI / Data** | ![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white) ![Spring AI](https://img.shields.io/badge/Spring_AI-6DB33F?style=flat-square&logo=spring&logoColor=white) ![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6B35?style=flat-square) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) |
| **Database** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white) ![SQLite](https://img.shields.io/badge/SQLite-07405E?style=flat-square&logo=sqlite&logoColor=white) |
| **Infra** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![Apache Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white) |
-->

---

### 💊 뭐냑 (AMApill) — MSA 기반 복약 관리 플랫폼

> ![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![Spring AI](https://img.shields.io/badge/Spring_AI-6DB33F?style=flat-square&logo=spring&logoColor=white) ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) ![Zustand](https://img.shields.io/badge/Zustand-20232a?style=flat-square&logo=react&logoColor=61DAFB) ![Apache Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) ![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=flat-square&logo=docker&logoColor=white)

부트캠프 3차 팀 프로젝트 (3인) | 2025.11 ~ 2025.12 | Backend 설계 · AI 연동 · Frontend 성능 최적화 담당

#### 주요 성과

| 항목 | Before | After | 개선 |
|------|--------|-------|------|
| 반복 조회 쿼리 수 | 197건 | 3건 | **98.5%↓** |
| 대시보드 초기 로딩 | 2.5초 | 1.5초 | **40%↓** |
| 약물 검색 재조회 | 외부 API/AI 재호출 | Redis cache hit | **동일 약물명 재검색 시 Redis cache hit으로 즉시 응답** |

#### 담당 구현

**Backend**
<br>
- **N+1 개선** — 복약 로그 조회 API 루프 내 `findById()` 단건 호출을 MyBatis IN절 + Map 일괄 조회로 재설계. 90건 기준 쿼리 197개 → 3개 (98.5%↓), 응답 3~5초 → 0.5초 이하 (Issue #112)
- **알림 Fail-Safe** — 알림 정책 오류 시 `shouldSend` 예외를 false로 처리(Block-by-default), DB 스키마 변경 없이 Redis State Overlay로 우회. 오발송·누락 양방향 해결, 감시 주기 30분 → 30초 외부화

**AI**

- **MFDS Fallback** — MFDS API 장애/타임아웃 시 Spring AI + OpenAI 기반 약물 검색 자동 전환. MFDS 결과 Redis 캐싱(TTL 24h), AI 생성 결과 별도 캐싱(TTL 7일)으로 동일 약물명 재검색 시 외부 호출 제거
- **AI 가드레일 (L1~L8)** — 프롬프트 인젝션 방어 및 API 비용 누수 방지를 위한 8계층 방어 아키텍처 직접 설계
<details>
<summary>
 가드레일 계층 상세 보기
</summary>

| Layer | 명칭 | 역할 |
|:-----:|:----:|------|
| L1 | Gateway Network | 네트워크 레벨 진입 차단 |
| L2 | Normalization | 공백·Zero-width·전각문자 정규화로 불가시 문자 기반 우회 인젝션 차단 (ICU4J) |
| L3 | Regex Guard | 표준 jailbreak 패턴(ignore/override/act-as 계열) Regex 탐지 및 스트라이크 누적 |
| L4 | Prompt Isolation | XML 태그 기반 입력 격리로 인젝션 차단 |
| L5 | AI Nano | gpt-4o-mini로 입력을 DRUG / SYMPTOM / GENERAL_CHAT 3분류, GENERAL_CHAT 판별 시 즉시 차단 |
| L6 | Core LLM | 검증 통과 요청만 메인 모델 실행 |
| L7 | Canary Token | 응답에 `CANARY_` 토큰 포함 감지 시 즉시 PERMANENT_BAN 처리 |
| L8 | Resilience | abuse/bust 로그 기반 이상 트래픽 탐지 |

> 흐름: Gateway → Normalization → Regex → Prompt Isolation → AI 분류 → Core LLM → Canary → Resilience

</details>
<br>

**Frontend**

- **대시보드 최적화** — `zustand/shallow` + `React.memo` + 스토어 캐싱으로 불필요한 리렌더링 제거. Playwright 측정 기준 초기 로딩 40%↓ (2.5초 → 1.5초), 탭 전환 Zero-Latency
  <br>

**기타**

- **PDF 리포트** — 지병/복약 목록 + 최근 30일 순응도(파이 차트) PDF 생성, OpenPDF 한글 폰트 임베딩


👉 [GitHub Organization](https://github.com/KOSA2025-FINAL-PROJECT-TEAM3)

---

### 🔍 Documind — AI 기반 문서 분석 RAG 시스템
> ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white) ![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6B35?style=flat-square) ![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white) ![SQLite](https://img.shields.io/badge/SQLite-07405E?style=flat-square&logo=sqlite&logoColor=white)

POC 프로젝트 (3인) | 2026.01



#### 담당 구현

### RAG / Retrieval Pipeline

- 문서 Q&A용 RAG는 업로드 문서를 chunk 단위로 분리한 뒤, embedding 기반 인메모리 index를 생성해 검색하도록 구현했습니다.
- Target Optimizer의 best-practice 검색에는 ChromaDB 저장소와 BM25를 결합한 hybrid retrieval을 적용했습니다.
- 검색 점수는 embedding similarity 0.6, BM25 0.4 가중치로 계산했습니다.
- ChromaDB 조회 실패나 조건 미일치 상황에서는 예외 전파를 줄이고 빈 결과 또는 전체 컬렉션 재조회로 처리하는 fail-soft 흐름을 적용했습니다.

#### Architecture

- **Provider Abstraction** — Ollama / Gemini / Claude / OpenAI 4개 프로바이더를 추상화 레이어로 통합. 프로바이더 교체 시 RAG 핵심 로직 수정 없이 전환 가능한 구조 설계

#### Data

- **Metadata Management** — SQLite 기반 문서 메타데이터 관리 체계 도입. 문서 추적성 확보 및 운영 관점 관리 효율 향상
- **Document Stability** — 한글 UTF-8 BOM 처리 및 PDF 출력 호환성 개선으로 문서 파이프라인 안정성 강화

👉 [GitHub](https://github.com/gmkoo-d3v/AIPOC)

---

### 🧭 ZeniManager — AI 기반 취업 지원 상담 관리 시스템
> ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) ![TailwindCSS](https://img.shields.io/badge/TailwindCSS-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white) ![Electron](https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=electron&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=openai&logoColor=white)

팀 프로젝트 (5인) | 2026.03

#### 담당 구현

#### AI / Vector Search

- **유사 성공사례 검색 설계** — 상담사 상세 페이지에서 내담자 프로필 기반 유사 취업 성공사례를 추천하는 vector retrieval 구현. pgvector 도입 결정부터 임베딩 설계까지 단독 담당
- **임베딩 설계** — 연령대·학력·전공·희망직무 4축을 핵심 매칭 키로 선정. 나이는 `age_decade`(연령대)로 변환해 과적합 방지, 희망직무 3개 항목을 단일 축으로 통합
- **PII 평탄화** — 이름·상세 식별자를 마스킹하고 quasi-identifier(연령+학교+전공 조합) 재식별 리스크를 고려한 snapshot 구조 설계

#### Auth

- **Electron 인증 안정화** — 재로그인/유휴 상태 이후 Supabase 세션 끊김 이슈 원인 분석 및 세션 복구 흐름 정비
#### Dashboard

- **KPI/통계** — 상담사별 실적 KPI 및 통계 기능 설계·구현, 대시보드 API 모듈 분리로 유지보수성 개선
- **캘린더·메모** — 일정 관리 캘린더 및 포스트잇 메모 기능 구현


👉 [GitHub Repository](https://github.com/SuranS2/ZeniManager)

---

### 🧭 CodeGuide — LLM 협업 워크플로우 프레임워크

> ![Markdown](https://img.shields.io/badge/Markdown-000000?style=flat-square&logo=markdown&logoColor=white)

개인 프로젝트 
LLM을 코드 타이핑에 위임하고, 설계·의사결정·검증은 직접 주도하는 워크플로우 체계


#### 핵심 구조

- **문서 중심 워크플로우** — `task / plan / decisions / shadow` 4축으로 작업 이력·의사결정·협업 상태를 파일 기반으로 체계화. 세션 단위 컨텍스트 한계를 workspace docs로 보완
- **Plan Ping-Pong Loop** — 복수 LLM 모델이 plan을 상호 검토하며 점진 수렴하는 설계 검증 루프. 단일 모델 편향 방지
- **멀티 에이전트 역할 분리** — supervising lead architect(메인) + 계획·리뷰·구현·검증 서브에이전트로 협업 구조 정의
- **5축 의사결정 기록** — Why / What / How / Where / Verify 기준으로 모든 설계 결정을 추적 가능하게 문서화

  👉 [GitHub Repository](https://github.com/gmkoo-d3v/codeguide)

---

## 📊 GitHub Stats

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats-private-sable.vercel.app/api?username=gmkoo-d3v&show_icons=true&theme=tokyonight" />
    <img height="180" src="https://github-readme-stats-private-sable.vercel.app/api?username=gmkoo-d3v&show_icons=true" alt="GitHub Stats" />
  </picture>
</div>

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="github-readme-stats-private-sable.vercel.app/api/top-langs/?username=gmkoo-d3v&layout=compact&theme=tokyonight" />
    <img height="180" src="https://github-readme-stats-private-sable.vercel.app/api/top-langs/?username=gmkoo-d3v&layout=compact" alt="Top Langs" />
  </picture>
</div>

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-streak-stats.herokuapp.com/?user=gmkoo-d3v&theme=tokyonight" />
    <img src="https://github-readme-streak-stats.herokuapp.com/?user=gmkoo-d3v" alt="GitHub Streak" />
  </picture>
</div>

## 🏆 GitHub Trophies
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/trophy-dark.svg" />
  <img src="./assets/trophy-light.svg" alt="GitHub Trophies" />
</picture>

## 📈 Activity Graph

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-activity-graph.vercel.app/graph?username=gmkoo-d3v&theme=tokyo-night&area=true" />
    <img src="https://github-readme-activity-graph.vercel.app/graph?username=gmkoo-d3v&theme=minimal&area=true" width="100%" alt="Activity Graph" />
  </picture>
</div>

## 🐍 Contribution Snake

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/gmkoo-d3v/gmkoo-d3v/output/github-snake-dark.svg" />
    <img src="https://raw.githubusercontent.com/gmkoo-d3v/gmkoo-d3v/output/github-snake.svg" alt="Snake animation" />
  </picture>
</div>

## ✍️ Random Dev Quote

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://quotes-github-readme.vercel.app/api?type=vertical&theme=tokyonight" />
    <img src="https://quotes-github-readme.vercel.app/api?type=vertical&theme=light" alt="Random Dev Quote" />
  </picture>
</div>

---

<!--### 블로그
👉 [GitHub](https://gmkoo-d3v.github.io/blog/) -->
