# MCP - AI 기반 클라우드 리소스 예측 플랫폼

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)](https://fastapi.tiangolo.com/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.15+-orange.svg)](https://www.tensorflow.org/)
[![Claude](https://img.shields.io/badge/Claude-3.5%20Sonnet-purple.svg)](https://www.anthropic.com/)
[![React](https://img.shields.io/badge/React-18-blue.svg)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue.svg)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-5-blueviolet.svg)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3-38bdf8.svg)](https://tailwindcss.com/)
[![Vercel](https://img.shields.io/badge/Deploy-Vercel-black.svg)](https://vercel.com/)

자연어 설명과 GitHub 리포지토리 URL만으로
클라우드 리소스 사용량을 예측하고, 적절한 인스턴스 타입과
예상 비용까지 추천해 주는 엔드 투 엔드 AI 플랫폼입니다.

> 예시:  
> `"피크타임에 5000명 정도 사용할 것 같아요"` + `https://github.com/owner/repo`  
> → CPU/Memory 24시간 예측 + 권장 Flavor + 예상 비용 + 시계열 그래프

---

## 프로젝트 개요

이 레포지토리는 두 개의 서브 프로젝트로 구성된
클라우드 리소스 예측/이상탐지 플랫폼입니다.

- `mcp_core/`  
  AI 예측 엔진 및 Backend API  
  (FastAPI, TensorFlow LSTM, MySQL, Claude, MCP 서버)
- `mcp_web/`  
  React + TypeScript 기반 웹 대시보드  
  (Vite, Tailwind CSS, shadcn/ui, TanStack Query, Vercel)

백엔드에서 자연어와 GitHub 메타데이터를 기반으로
CPU/Memory 사용량을 24시간 단위로 예측하고,  
프론트엔드에서는 이를 시각화하여 운영자가
쉽게 인프라 규모를 결정할 수 있도록 도와줍니다.

---

## 주요 기능

- **자연어 + GitHub URL 기반 예측**  
  - 사용자 입력과 리포지토리 메타데이터만으로 리소스 사용량 자동 예측
- **24시간 시계열 리소스 예측**  
  - LSTM 모델과 Baseline 모델을 이용한 CPU/Memory 예측
- **Flavor / 비용 추천**  
  - 예측 결과를 바탕으로 권장 인스턴스 타입 및 예상 일일 비용 계산
- **이상 탐지 및 알림**  
  - Z-Score 기반 이상 패턴 감지 및 Discord Webhook 알림
- **프로젝트/서비스 단위 관리**  
  - 여러 서비스의 예측 결과를 한 화면에서 모니터링 (웹 대시보드)
- **(선택) MySQL 이력 저장**  
  - 예측 요청/결과를 DB에 저장하여 히스토리 분석 가능

---

## 아키텍처

플랫폼은 다음과 같은 흐름으로 동작합니다.

```text
사용자 (웹)
  └─ GitHub URL + 자연어 입력
        │
        ▼
Frontend (mcp_web, React)
  └─ POST /api/predict 또는 /plans
        │
        ▼
Backend API (FastAPI)
  ├─ GitHub 메타데이터 수집
  ├─ Claude API 호출 (자연어 → MCPContext JSON)
  └─ MCP Core에 예측 요청
        │
        ▼
MCP Core (FastAPI + TensorFlow)
  ├─ LSTM / Baseline 예측
  ├─ Policy Engine (정규화/Clamp)
  ├─ Anomaly Detection (Z-Score)
  └─ Discord 알림 / MySQL 저장
```

자세한 구조와 API 스펙은 각 서브 프로젝트의 README와
`mcp_core/docs/` 디렉토리 문서를 참고하면 됩니다.

---

## 레포지토리 구조

```text
mcp/
├── mcp_core/   # 예측 엔진 & Backend API
└── mcp_web/    # 웹 대시보드 (Frontend)
```

- `mcp_core/README.md`  
  - LSTM 기반 예측 엔진, Backend API, MCP Analyzer(Claude Desktop 연동),  
    MySQL 스키마, Docker 배포 등 백엔드 전반을 설명합니다.
- `mcp_web/README.md`  
  - React + TypeScript 기반 프론트엔드 구조, Vercel 배포,  
    인증/프로젝트/예측 화면 등 UX 흐름을 설명합니다.

---

## 빠른 시작

### 1. MCP Core (Backend / AI 엔진)

자세한 내용은 `mcp_core/README.md`를 확인하세요.

```bash
cd mcp_core
# 의존성 설치
poetry install

# FastAPI 서버 실행 (예: 포트 8000)
poetry run uvicorn app.main:app --reload
```

### 2. MCP Web (Frontend)

자세한 내용은 `mcp_web/README.md`를 확인하세요.

```bash
cd mcp_web
npm install
npm run dev
```

---

## 로드맵

- 더 다양한 클라우드 프로바이더의 인스턴스 타입 지원
- 예측 모델 고도화 (멀티 변량, Attention 등)
- CI/CD 파이프라인 및 자동 배포 워크플로우 강화
- 팀 단위 권한/프로젝트 관리 기능

---

## 기여

이 레포지토리는 실험적인 AI 기반 클라우드 리소스
예측/운영 도메인을 탐구하기 위한 프로젝트입니다.  
아이디어, 이슈, PR 모두 환영합니다.

자세한 사용법과 구현 세부 사항은 각 서브 프로젝트의 README를 참고해주세요.
