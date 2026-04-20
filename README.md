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
> ![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white) ![Spring AI](https://img.shields.io/badge/Spring_AI-6DB33F?style=flat-square&logo=spring&logoColor=white) ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) ![Zustand](https://img.shields.io/badge/Zustand-20232a?style=flat-square&logo=react&logoColor=61DAFB) ![MUI](https://img.shields.io/badge/MUI-0081CB?style=flat-square&logo=mui&logoColor=white) ![Apache Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white) ![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white) ![PostgreSQL_Vector_Store](https://img.shields.io/badge/PostgreSQL_Vector_Store-316192?style=flat-square&logo=postgresql&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white) ![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=flat-square&logo=docker&logoColor=white)

부트캠프 3차 팀 프로젝트 (3인) | 2025.11 ~ 2025.12 (7주 MVP)

**담당 역할 및 구현 내용**
- **AI 연동/가용성 개선**: MFDS 공공 API 장애/타임아웃 시 `Spring AI + OpenAI` 기반 약물 검색 Fallback 구현, `GET /api/medications/search/ai` 엔드포인트 추가로 자동/수동 AI 검색 흐름 통합.
- **AI 보안 / 가드레일 설계**: 프롬프트 인젝션 방어 및 API 비용 누수를 막기 위해 8계층 다층 방어(L1~L8) 아키텍처 고안. 의도 판별(Intent) 캐싱, Redis 카운터 기반 3진 아웃제, Canary Token(킬스위치)을 융합하여 악성 트래픽을 시스템적으로 원천 차단.
- **프론트엔드 주도 개발**: React + Vite + Zustand 기반 구조 설계/구현, `zustand/shallow` + `React.memo` + 스토어 캐싱으로 대시보드 초기 로딩 **40% 단축** 및 탭 전환 **Zero-Latency** 구현.
- **백엔드 기능 개발**: 가족 그룹/초대 관리 및 질병 정보 도메인(CRUD) 중심 비즈니스 로직 구현.
- **리포트 기능**: 지병/복약 목록/최근 30일(당일 제외) 순응도(약별·전체 파이 차트) 포함 PDF 생성, 한글 폰트 임베딩(OpenPDF)·
- **성능 최적화**: 백엔드 예외 처리(Fail-Safe): 알림 시스템 정책 오류 시, DB 스키마 롤백 없이 Redis 기반 상태 오버레이(State Overlay) 로직을 우회 구축하여 무중단 이슈 해결.
- **캐싱**: Redis(TTL 24h) 적용으로 AI 약물 검색 속도 **70% 향상** (`2.5s -> 0.75s`).

> 💡 **한 줄 요약**
> 가족 중심 어르신 복약 플랫폼 AMApill에서, 8계층 AI 가드레일 보안설계·MFDS 장애 대응 AI Fallback·프론트 성능 최적화(40%)·Redis 캐싱(70%)·N+1 개선(98.5%)을 중심으로 Full-Stack 기능을 구현했습니다.

👉 [GitHub Organization](https://github.com/KOSA2025-FINAL-PROJECT-TEAM3)

---

### 🔍 Documind — AI 기반 문서 분석 RAG 시스템
> ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white) ![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6B35?style=flat-square) ![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white) ![SQLite](https://img.shields.io/badge/SQLite-07405E?style=flat-square&logo=sqlite&logoColor=white)

POC 프로젝트 (3인) | 2026.01

- 가중치 기반 하이브리드 검색(Embedding 0.6 + BM25 0.4)으로 검색 정확도 향상
- 벡터 스토어 장애 상황에 대비한 폴백(fallback) 경로 구축으로 서비스 연속성 확보
- 한글 UTF-8 BOM 및 PDF 출력 호환성 개선으로 문서 처리 안정성 강화
- SQLite 기반 메타데이터 관리 체계 도입으로 문서 추적성과 운영 효율 향상


👉 [GitHub](https://github.com/gmkoo-d3v/AIPOC)

---

### 🧭 ZeniManager — AI 기반 취업 지원 상담 관리 시스템
> ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) ![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white) ![TailwindCSS](https://img.shields.io/badge/TailwindCSS-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white) ![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=flat-square&logo=supabase&logoColor=white) ![Electron](https://img.shields.io/badge/Electron-47848F?style=flat-square&logo=electron&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=openai&logoColor=white)

팀 프로젝트 (5인) | 2026.03

**담당 역할 및 구현 내용**
- **대시보드 고도화**: 상담사별 KPI/통계, 캘린더, 메모(포스트잇) 기능을 설계·구현하고 대시보드 API 모듈을 분리해 업무 흐름과 유지보수성을 함께 개선.
- **유사 성공사례 검색**: 상담자 상세 페이지에서 `Supabase pgvector` 기반 취업 성공사례 유사검색 POC를 구현하고, 연령대/이름 마스킹 규칙을 적용해 개인정보 노출을 최소화.
- **보안/권한 강화**: 상담사 소유 boundary 기반 데이터 접근 제어를 적용하고, 상담사별 클라이언트 API 호출 보안을 강화.
- **인증 안정화**: Electron 환경에서 Supabase 세션 복구 흐름을 정비해 재로그인/유휴 상태 이후 인증 끊김 이슈를 완화.

> 💡 **한 줄 요약**
> ZeniManager에서 상담사 대시보드, 메모/캘린더, 유사 성공사례 벡터 검색, 권한 보안, Electron 세션 안정화를 중심으로 프론트엔드와 운영 품질을 고도화했습니다.

👉 [GitHub Repository](https://github.com/SuranS2/ZeniManager)

---
### 🧭 CodeGuide — 프로덕션급 아키텍처·코드 품질 엔지니어링 스킬
 
개발 생산성/품질 표준화 스킬 | Skills

**구현 내용**
- **아키텍처 가이드 제공**: SOLID, DRY, KISS, YAGNI 기반으로 설계, 리팩터링, 리뷰, 디버깅 전반의 일관된 판단 기준을 제공합니다.
- **문서 중심 워크플로우 설계**: docs/task, plan, decisions, report, shadow, orchestration를 기준으로 작업 이력, 의사결정, 협업 상태를 체계화합니다.
- **멀티 에이전트 협업 체계 정립**: 메인 스레드는 supervising lead architect, 서브에이전트는 계획, 리뷰, 구현, 검증 역할로 분리해 협업 효율을 높입니다.
- **품질 검증 기준 강화**: 문서 검증, runtime validation, config-first, deterministic test 원칙을 통해 변경 안정성과 운영 신뢰도를 높입니다.

> 💡 **한 줄 요약**
> CodeGuide는 파일 기반 workspace docs를 system of record로 삼아, 세션 단위 컨텍스트 한계를 완화하고 의사결정 추적, 멀티 에이전트 협업, 검증 기준을 일관되게 운영하는 컨텍스트 엔지니어링 프레임워크입니다.

👉 [GitHub Repository](https://github.com/gmkoo-d3v/codeguide)

---

## 📊 GitHub Stats

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats-private-zeta-nine.vercel.app/api?username=gmkoo-d3v&show_icons=true&theme=tokyonight" />
    <img height="180" src="https://github-readme-stats-private-zeta-nine.vercel.app/api?username=gmkoo-d3v&show_icons=true" alt="GitHub Stats" />
  </picture>
</div>

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="github-readme-stats-private-zeta-nine.vercel.app/api/top-langs/?username=gmkoo-d3v&layout=compact&theme=tokyonight" />
    <img height="180" src="https://github-readme-stats-private-zeta-nine.vercel.app/api/top-langs/?username=gmkoo-d3v&layout=compact" alt="Top Langs" />
  </picture>
</div>

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-streak-stats.herokuapp.com/?user=gmkoo-d3v&theme=tokyonight" />
    <img src="https://github-readme-streak-stats.herokuapp.com/?user=gmkoo-d3v" alt="GitHub Streak" />
  </picture>
</div>

## 🏆 GitHub Trophies

<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github-profile-trophy-vercel.vercel.app/?username=gmkoo-d3v&margin-w=15&theme=discord" />
    <img src="https://github-profile-trophy-vercel.vercel.app/?username=gmkoo-d3v&margin-w=15" alt="GitHub Trophies" />
  </picture>
</div>

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
