# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

**LogWatch Admin** - 실시간 로그 수집, 분석 및 모니터링을 위한 웹 기반 어드민 대시보드

## 명령어

```bash
npm install        # 의존성 설치
npm run dev        # 개발 서버 시작 (Turbopack, localhost:3000)
npm run build      # 프로덕션 빌드
npm run start      # 프로덕션 서버 시작
npm run lint       # ESLint 실행
```

## 기술 스택

- **프레임워크**: Next.js 16 (App Router)
- **언어**: TypeScript
- **스타일링**: Tailwind CSS v4
- **번들러**: Turbopack

## 아키텍처

### 라우팅 구조

Next.js 16 App Router 기반의 FE + BE 통합 프로젝트.

**라우트 그룹:**
- **`(dashboard)`** - 대시보드, 로그 검색, 알림 관리, 설정 페이지. `Sidebar` + `Header` 레이아웃 적용. URL에는 나타나지 않음.
- **`(auth)`** - 로그인/회원가입 페이지. 사이드바 없는 중앙 정렬 레이아웃. URL에는 나타나지 않음.
- **`api/`** - 백엔드 API 라우트 핸들러. 각 엔드포인트는 `route.ts`에서 HTTP 메서드 함수를 export.

**주요 라우트:**
| 경로 | 설명 | 레이아웃 |
|------|------|----------|
| `/` | 대시보드 메인 (실시간 로그 스트림, 통계) | Sidebar + Header |
| `/logs` | 로그 목록 및 검색 | Sidebar + Header |
| `/alerts` | 알림 관리 | Sidebar + Header |
| `/settings` | 시스템 설정 | Sidebar + Header |
| `/login` | 로그인 | 중앙 정렬 |
| `/register` | 회원가입 | 중앙 정렬 |

### 디렉토리 구조

```
src/
├── app/                  # Next.js App Router (페이지, 레이아웃, API 라우트)
│   ├── (dashboard)/      # 대시보드 라우트 그룹
│   ├── (auth)/           # 인증 라우트 그룹
│   └── api/              # API 라우트 핸들러
├── components/
│   ├── ui/               # 재사용 UI 프리미티브 (Button, Input, Card 등)
│   ├── layout/           # 셸 컴포넌트 (Sidebar, Header)
│   └── features/         # 기능별 복합 컴포넌트
├── lib/                  # 공유 유틸리티, 상수, 헬퍼
├── hooks/                # 커스텀 React 훅
├── types/                # TypeScript 타입 정의
├── services/             # API 클라이언트 함수 / 외부 서비스 연동
└── styles/               # 추가 글로벌 스타일 (Tailwind 외)
```

### API 엔드포인트

| 메서드 | 경로 | 설명 |
|--------|------|------|
| `GET` | `/api/logs` | 로그 목록 조회 (필터링, 페이지네이션) |
| `GET` | `/api/logs/:id` | 로그 상세 조회 |
| `GET` | `/api/alerts` | 알림 규칙 목록 |
| `POST` | `/api/alerts` | 알림 규칙 생성 |
| `GET` | `/api/stats` | 대시보드 통계 데이터 |
| `POST` | `/api/auth/login` | 로그인 |
| `POST` | `/api/auth/register` | 회원가입 |

### 주요 기능

**대시보드**
- 실시간 로그 스트림 모니터링
- 로그 레벨별 필터링 (ERROR, WARN, INFO, DEBUG)
- 시간대별 로그 발생 추이 차트
- 핵심 지표 요약 카드

**로그 검색 및 분석**
- 키워드 기반 로그 검색
- 날짜/시간 범위 필터
- 로그 소스별 분류
- 상세 로그 뷰어

**알림 관리**
- 임계값 기반 알림 규칙 설정
- 알림 이력 조회
- 알림 채널 관리 (이메일, 슬랙 등)

**시스템 관리**
- 사용자 계정 관리
- 로그 수집 소스 설정
- 보존 정책 관리

## 코딩 컨벤션

- **import 별칭**: `@/*`는 `src/*`에 매핑 (예: `import { Sidebar } from "@/components/layout/Sidebar"`)
- **컴포넌트 export**: named export 사용, default export 사용하지 않음 (예: `export function LogTable() { ... }`)
- **API 응답**: `next/server`의 `NextResponse.json()`으로 반환
- **사용자 대면 문자열**: 한국어 사용

## 사전 요구사항

- Node.js 18 이상
- npm

## AntiGravity Rules (명령어 사용 규칙)

다음 명령어들은 시스템 안전을 위해 사용이 제한되거나 주의가 필요합니다.

### 🚫 금지된 명령어 (절대 실행 금지)
- `rm -rf` (재귀적 강제 삭제)
- `sudo` (관리자 권한 실행)
- `curl`, `wget` (외부 리소스 다운로드 - 승인 필요)
- `git push --force` (강제 푸시)
- `git reset --hard` (강제 리셋)
- `gh repo delete` (리포지토리 삭제)
- `.env` 및 `secrets` 관련 파일 읽기

### ⚠️ 주의가 필요한 명령어 (사용자 승인 권장)
- `rm` (파일 삭제)
- `npm install` (패키지 설치 - 사전에 알질 것)
