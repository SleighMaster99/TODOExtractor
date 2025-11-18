# Todo Extractor Product Requirements Document (PRD)

## 1. Goals and Background Context

### 1.1 Goals

- 코드베이스에서 TODO, FIXME 등 주석 마커를 자동으로 추출하여 작업 부채 가시화
- 스프린트 계획 및 리팩토링 우선순위 결정 지원
- Visual Studio 2022 개발자가 쉽게 사용할 수 있는 도구 제공
- CSV/Markdown/JSON 형식의 리포트 생성으로 다양한 활용 지원

### 1.2 Background Context

개발 프로젝트가 성장함에 따라 코드 내 TODO, FIXME 등의 주석이 산재하게 됩니다. 이러한 작업 항목들을 수동으로 추적하는 것은 비효율적이며, 누락되기 쉽습니다. Todo Extractor는 코드베이스를 재귀적으로 스캔하여 지정된 마커가 포함된 주석을 자동으로 수집하고 정리된 리포트로 출력합니다.

이 도구를 통해 팀은 남아있는 기술 부채와 작업 항목을 한눈에 파악할 수 있으며, 이를 기반으로 스프린트 계획과 리팩토링 우선순위를 효과적으로 결정할 수 있습니다.

### 1.3 Change Log

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2024-01-01 | 1.0 | 최초 PRD 작성 | Business Analyst |

---

## 2. Requirements

### 2.1 Functional Requirements

- **FR1**: 지정된 디렉터리를 재귀적으로 스캔하여 소스 파일 탐색
- **FR2**: 설정된 마커(예: `TODO:`, `FIXME:`, `!`)가 포함된 주석 라인 추출
- **FR3**: 마커는 반드시 `//`로 시작하는 주석에서만 추출 (예: `//TODO:`, `//FIXME:`, `//!`)
- **FR4**: 단순히 마커 텍스트가 포함된 것이 아닌, `//마커`로 시작하는 라인만 추출
- **FR5**: 여러 개의 마커를 설정에서 추가/관리 가능
- **FR6**: 추출 결과를 CSV, Markdown, JSON 형식으로 출력
- **FR7**: 기본 제공 마커: `TODO:`, `FIXME:`, `HACK:`, `XXX:`, `BUG:`
- **FR8**: 스캔 대상 파일 확장자 설정 가능 (기본: `.cpp`, `.h`, `.hpp`, `.c`, `.cs`, `.py`, `.js`, `.ts`)
- **FR9**: 제외할 디렉터리 설정 가능 (기본: `node_modules`, `.git`, `bin`, `obj`, `build`)
- **FR10**: 추출 결과에 파일명, 라인 번호, 마커 유형, 내용 포함
- **FR11**: 리포트를 Markdown 파일로 출력하여 가독성 확보
- **FR12**: Visual Studio 확장 프로그램에서 UI를 통한 마커 추가/삭제 기능 제공

### 2.2 Non-Functional Requirements

- **NFR1**: C++17 표준 라이브러리만 사용 (외부 의존성 없음)
- **NFR2**: 1일 내 개발 가능한 MVP 범위로 제한
- **NFR3**: CLI 프로그램으로 먼저 개발, 이후 VS 확장으로 확장
- **NFR4**: 대용량 코드베이스(10만+ 파일)에서도 합리적인 시간 내 처리
- **NFR5**: Windows 환경에서 실행 가능한 exe 파일 생성
- **NFR6**: 설정 파일은 JSON 또는 YAML 형식으로 저장

---

## 3. User Interface Design Goals

### 3.1 Overall UX Vision

CLI 버전은 간단한 명령줄 인터페이스로, Visual Studio 확장 버전은 직관적인 설정 UI와 결과 뷰어를 제공합니다.

### 3.2 Key Interaction Paradigms

- **CLI**: 명령줄 인자로 옵션 전달, 설정 파일 로드 지원
- **VS 확장**: 메뉴/툴바에서 실행, 설정 다이얼로그, 결과 패널

### 3.3 Core Screens and Views

**CLI 버전:**
- 콘솔 출력 (진행 상황, 결과 요약)

**VS 확장 버전:**
- 설정 다이얼로그 (마커 관리, 확장자, 제외 디렉터리)
- 마커 추가/삭제 UI
- 결과 패널 (추출된 항목 목록)
- 리포트 미리보기

### 3.4 Accessibility

None (MVP 버전)

### 3.5 Branding

특별한 브랜딩 요구사항 없음

### 3.6 Target Device and Platforms

- Windows Desktop (Visual Studio 2022)
- CLI는 크로스 플랫폼 가능하나 1차 타겟은 Windows

---

## 4. Technical Assumptions

### 4.1 Repository Structure

Monorepo - CLI와 VS 확장을 하나의 저장소에서 관리

### 4.2 Service Architecture

- **CLI 프로그램**: 단일 실행 파일 (Monolith)
- **VS 확장**: VSIX 패키지

### 4.3 Testing Requirements

Unit Only - 핵심 파서 및 추출 로직에 대한 유닛 테스트

### 4.4 Additional Technical Assumptions

- 언어: C++17 (CLI), C# (VS 확장)
- 빌드 시스템: CMake (CLI), MSBuild (VS 확장)
- 설정 저장: JSON 형식
- 출력 인코딩: UTF-8
- 주석 형식: 단일 라인 주석 (`//`)만 지원 (MVP)

---

## 5. Epic List

### Epic 1: CLI 핵심 기능 개발
코드 스캐너와 마커 추출 엔진을 C++17로 구현하여 기본 CLI 도구 완성

### Epic 2: Visual Studio 확장 개발
CLI 기능을 VS 확장으로 래핑하고 설정 UI 및 결과 뷰어 구현

---

## 6. Epic Details

### Epic 1: CLI 핵심 기능 개발

**Goal**: C++17 표준 라이브러리만 사용하여 코드베이스를 스캔하고 마커를 추출하는 CLI 도구를 개발합니다. 사용자는 명령줄에서 실행하여 Markdown 리포트를 생성할 수 있습니다.

#### Story 1.1: 프로젝트 설정 및 기본 구조

**As a** 개발자,
**I want** 프로젝트 기본 구조와 빌드 시스템을 설정하고 싶습니다,
**so that** 이후 기능 개발을 위한 기반을 마련할 수 있습니다.

**Acceptance Criteria:**
1. CMakeLists.txt 생성 및 C++17 설정
2. 기본 main.cpp 생성 및 빌드 확인
3. 프로젝트 디렉터리 구조 수립 (src, include, tests)

#### Story 1.2: 설정 파일 파서 구현

**As a** 사용자,
**I want** JSON 설정 파일에서 마커, 확장자, 제외 디렉터리를 읽고 싶습니다,
**so that** 추출 동작을 커스터마이즈할 수 있습니다.

**Acceptance Criteria:**
1. JSON 파서 구현 (표준 라이브러리만 사용 또는 단순 파서)
2. 기본 설정값 정의 (기본 마커, 확장자, 제외 디렉터리)
3. 설정 파일 로드 및 병합 기능
4. 다중 마커 지원 (배열 형태로 여러 마커 정의)

#### Story 1.3: 디렉터리 재귀 스캐너 구현

**As a** 사용자,
**I want** 지정된 디렉터리를 재귀적으로 스캔하고 싶습니다,
**so that** 모든 소스 파일을 탐색할 수 있습니다.

**Acceptance Criteria:**
1. `std::filesystem`을 사용한 재귀 디렉터리 탐색
2. 설정된 확장자 필터링
3. 제외 디렉터리 건너뛰기
4. 탐색된 파일 목록 반환

#### Story 1.4: 마커 추출 엔진 구현

**As a** 사용자,
**I want** 파일에서 `//마커`로 시작하는 주석을 추출하고 싶습니다,
**so that** TODO 항목을 수집할 수 있습니다.

**Acceptance Criteria:**
1. 파일을 라인 단위로 읽기
2. `//`로 시작하는 라인 감지
3. `//` 다음에 설정된 마커로 시작하는지 확인 (예: `//TODO:`, `//FIXME:`)
4. 단순히 마커 텍스트 포함이 아닌 `//마커`로 시작하는 경우만 추출
5. 파일명, 라인 번호, 마커 유형, 내용 저장
6. 공백 처리 (예: `// TODO:`, `//TODO:` 모두 지원)

#### Story 1.5: Markdown 리포트 생성기

**As a** 사용자,
**I want** 추출 결과를 Markdown 파일로 출력하고 싶습니다,
**so that** 가독성 좋은 리포트를 얻을 수 있습니다.

**Acceptance Criteria:**
1. 마커 유형별 그룹화
2. 테이블 형식 출력 (파일, 라인, 내용)
3. 요약 정보 포함 (총 개수, 마커별 개수)
4. 파일 경로를 상대 경로로 표시

#### Story 1.6: CLI 인터페이스 및 인자 파싱

**As a** 사용자,
**I want** 명령줄 인자로 옵션을 전달하고 싶습니다,
**so that** 설정 파일 없이도 빠르게 실행할 수 있습니다.

**Acceptance Criteria:**
1. 인자 파싱 구현 (`-d`: 디렉터리, `-o`: 출력 파일, `-c`: 설정 파일)
2. 도움말 출력 (`-h`, `--help`)
3. 기본값 처리
4. 오류 메시지 출력

#### Story 1.7: CSV 및 JSON 출력 지원

**As a** 사용자,
**I want** 결과를 CSV와 JSON으로도 출력하고 싶습니다,
**so that** 다른 도구와 연동할 수 있습니다.

**Acceptance Criteria:**
1. CSV 출력 구현
2. JSON 출력 구현
3. 출력 형식 선택 옵션 (`-f md|csv|json`)

---

### Epic 2: Visual Studio 확장 개발

**Goal**: CLI 기능을 Visual Studio 2022 확장으로 통합하고, 사용자 친화적인 UI를 통해 마커 관리 및 결과 확인 기능을 제공합니다.

#### Story 2.1: VS 확장 프로젝트 설정

**As a** 개발자,
**I want** Visual Studio 확장 프로젝트를 생성하고 싶습니다,
**so that** VS 환경에서 도구를 사용할 수 있습니다.

**Acceptance Criteria:**
1. VSIX 프로젝트 생성
2. 기본 메뉴 명령 추가
3. VS 2022 대상 설정

#### Story 2.2: 설정 UI 구현

**As a** 사용자,
**I want** UI를 통해 마커를 추가/삭제하고 싶습니다,
**so that** 코드 수정 없이 설정을 변경할 수 있습니다.

**Acceptance Criteria:**
1. 설정 다이얼로그 구현
2. 마커 목록 표시 및 추가/삭제 버튼
3. 확장자 및 제외 디렉터리 설정
4. 설정 저장/로드

#### Story 2.3: CLI 연동 및 결과 표시

**As a** 사용자,
**I want** VS에서 스캔을 실행하고 결과를 보고 싶습니다,
**so that** IDE를 벗어나지 않고 작업할 수 있습니다.

**Acceptance Criteria:**
1. CLI 실행 또는 핵심 로직 재사용
2. 결과 패널에 추출 항목 표시
3. 항목 클릭 시 해당 파일/라인으로 이동

---

## 7. Checklist Results Report

*(PRD 완성 후 PM 체크리스트 실행 예정)*

---

## 8. Next Steps

### 8.1 Architect Prompt

이 PRD를 기반으로 Todo Extractor의 아키텍처 문서를 작성해 주세요. C++17 CLI 프로그램과 Visual Studio 확장의 구조, 핵심 컴포넌트, 데이터 흐름을 정의해 주세요. 특히 마커 추출 로직(`//마커`로 시작하는 라인만 추출)의 상세 설계를 포함해 주세요.
