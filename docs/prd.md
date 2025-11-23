# Todo Extractor Product Requirements Document (PRD)

## Goals and Background Context

### Goals

- Visual Studio 2022 개발자들이 코드 내 TODO 주석을 수동으로 찾고 이슈화하는 시간을 90% 단축 (30분 → 3분)
- 코드베이스 전체에 흩어진 작업 항목의 가시성을 확보하고 누락률을 20% 이하로 감소
- GitHub/GitLab과의 양방향 연동으로 개발 워크플로우를 끊지 않고 즉시 이슈 생성
- 커스터마이징 가능한 마커 시스템으로 팀별 작업 추적 방식을 유연하게 지원
- 6개월 내 1,000명 활성 사용자 확보 및 평균 평점 4.0 이상 유지
- 개발 생산성 향상과 기술 부채 최소화를 통한 코드 품질 개선

### Background Context

개발자들은 코딩 중 떠오르는 작업을 `//TODO:`, `//FIXME:` 등의 주석으로 남기는 것이 일반적입니다. 하지만 이러한 주석들은 코드베이스 전체에 흩어져 있어 관리가 어렵고, 수동으로 찾아서 이슈 트래커에 등록하는 과정이 번거롭고 시간이 소요됩니다. 결과적으로 중요한 작업이 코드 주석에 묻혀 잊혀지거나, 팀 차원에서 우선순위를 관리하고 할당하기 어려운 문제가 발생합니다.

Todo Extractor는 Visual Studio 2022에 긴밀하게 통합된 확장 프로그램으로, 이러한 문제를 해결합니다. 사용자 정의 마커 기반 주석 추출, GitHub/GitLab 자동 이슈 생성, 다양한 로컬 파일 형식 지원(MD, CSV, JSON, HTML, DB) 등의 기능을 통해 코드 내 작업 항목을 효율적으로 관리하고 추적할 수 있습니다. 기존의 "Find in Files" 수동 검색이나 단순 나열만 제공하는 Task List와 달리, Todo Extractor는 이슈 관리 시스템과의 완전한 통합을 제공하여 개발자가 코딩 흐름을 벗어나지 않고 작업을 관리할 수 있도록 합니다.

### Change Log

| Date | Version | Description | Author |
|------|---------|-------------|--------|
| 2025-11-23 | 0.1 | 초기 PRD 작성 시작 | PM (John) |
| 2025-11-23 | 1.0 | PRD 완성 - 모든 섹션 작성 완료 (Requirements, UI Goals, Technical Assumptions, 4 Epics with 32 Stories) | PM (John) |

## Requirements

### Functional Requirements

**FR1:** 사용자는 여러 개의 커스텀 마커(예: TODO, FIXME, OPTIMIZE, BUG)를 등록, 편집, 삭제할 수 있어야 하며, 각 마커에 중요도(높음/보통/낮음)를 설정할 수 있어야 한다.

**FR2:** 시스템은 마커 중복을 방지하는 유효성 검사를 수행해야 하며, 대소문자 구분 옵션을 제공해야 한다.

**FR3:** 시스템은 지정된 파일 경로를 스캔하여 `//마커` 패턴으로 시작하는 주석만 추출해야 한다.

**FR4:** 사용자는 파일 확장자 필터(예: .cs, .cpp, .js, .h)를 설정하여 특정 파일 타입만 스캔할 수 있어야 한다.

**FR5:** 사용자는 제외 디렉터리(예: bin, obj, node_modules, .git)를 설정하여 불필요한 경로를 스캔에서 제외할 수 있어야 한다.

**FR6:** 시스템은 GitHub API를 통해 추출된 TODO를 자동으로 GitHub 이슈로 생성할 수 있어야 하며, Personal Access Token 기반 인증을 지원해야 한다.

**FR7:** 시스템은 GitLab API를 통해 추출된 TODO를 자동으로 GitLab 이슈로 생성할 수 있어야 하며, Personal Access Token 기반 인증을 지원해야 한다.

**FR8:** 사용자는 이슈 생성 시 저장소 URL, 기본 라벨, 기본 담당자를 설정할 수 있어야 한다.

**FR9:** 시스템은 추출 결과를 Markdown(.md), CSV(.csv), JSON(.json), HTML(.html), SQLite 데이터베이스 형식으로 저장할 수 있어야 한다.

**FR10:** 사용자는 로컬 파일 저장 시 그룹화 방식(마커별/파일별/우선순위별)과 정렬 순서를 선택할 수 있어야 한다.

**FR11:** 시스템은 이슈 생성 또는 파일 저장 전에 추출된 항목을 확인하고 선택적으로 실행할 수 있는 미리보기 UI를 제공해야 한다.

**FR12:** 미리보기 UI는 일괄 선택/해제, 검색, 필터 기능을 포함해야 한다.

**FR13:** 시스템은 마커 설정, 검사 경로 설정, 출력 옵션, 이슈 트래커 연동 설정을 관리하는 직관적인 설정 UI를 제공해야 한다.

**FR14:** 설정은 JSON 형식으로 저장되어야 하며, 프로젝트별로 독립적으로 관리되어야 한다.

**FR15:** 시스템은 추출된 TODO 항목에 대해 파일 경로, 라인 번호, 마커 타입, 주석 내용을 표시해야 한다.

### Non-Functional Requirements

**NFR1:** 시스템은 10,000개 파일 규모의 프로젝트에서 스캔 시간이 30초 이내여야 한다.

**NFR2:** UI 응답 시간은 모든 작업에서 1초 이내여야 한다.

**NFR3:** 확장 프로그램의 메모리 사용량은 200MB를 초과하지 않아야 한다.

**NFR4:** API 토큰은 Windows Credential Manager에 암호화되어 저장되어야 하며, 평문으로 저장되어서는 안 된다.

**NFR5:** 모든 외부 API 통신은 HTTPS를 통해서만 이루어져야 한다.

**NFR6:** 시스템은 민감 정보(API 토큰, 사용자 정보)를 로그에 기록해서는 안 된다.

**NFR7:** 시스템은 Visual Studio 2022 버전 17.0 이상과 호환되어야 한다.

**NFR8:** 확장 프로그램은 Windows 플랫폼에서 작동해야 한다.

**NFR9:** 시스템은 첫 실행 후 5분 내에 첫 이슈를 생성할 수 있을 정도로 사용하기 쉬워야 하며, 초기 사용자의 80% 이상이 이를 달성해야 한다.

**NFR10:** GitHub/GitLab API rate limiting(시간당 5,000 요청)을 고려하여 배치 처리 및 오류 처리를 구현해야 한다.

**NFR11:** 시스템은 치명적 오류 발생 시 사용자에게 명확한 오류 메시지와 복구 방법을 제공해야 한다.

**NFR12:** Visual Studio의 출력 창(Output Window)을 통해 상세한 작업 로그를 제공해야 한다.

## User Interface Design Goals

### Overall UX Vision

Todo Extractor의 UI는 Visual Studio 2022의 네이티브 경험을 존중하며, 개발자가 코딩 흐름을 벗어나지 않고 자연스럽게 사용할 수 있어야 합니다. "설치 후 5분 내 첫 이슈 생성"이라는 목표를 달성하기 위해 직관적이고 단계적인 인터페이스를 제공합니다.

핵심 UX 원칙:
- **비침습성(Non-intrusive):** 개발 작업을 방해하지 않는 사이드바 또는 도구 창 기반 UI
- **점진적 공개(Progressive Disclosure):** 초보자에게는 간단한 기본 옵션, 고급 사용자에게는 상세 설정 제공
- **즉각적 피드백:** 스캔, 이슈 생성 등 모든 작업의 진행 상태와 결과를 실시간으로 표시
- **오류 허용(Error Tolerance):** 되돌리기 가능한 작업, 미리보기 기능으로 실수 방지

### Key Interaction Paradigms

**1. 마커 기반 워크플로우:**
- 설정 > 스캔 > 미리보기 > 실행의 명확한 단계
- 각 단계에서 뒤로 가기 가능

**2. 컨텍스트 메뉴 통합:**
- Solution Explorer에서 우클릭 → "Extract TODOs" 옵션으로 빠른 실행
- 현재 파일, 프로젝트, 전체 솔루션 범위 선택 가능

**3. 키보드 중심 네비게이션:**
- 모든 주요 기능에 키보드 단축키 제공 (예: Ctrl+Shift+T로 TODO 추출 실행)
- Tab 키로 순차 네비게이션, Enter로 실행

**4. Visual Studio 표준 패턴 준수:**
- WPF 기반 도구 창 (Tool Window)
- VS 테마(Light/Dark) 자동 감지 및 적용
- 아이콘과 색상은 VS 디자인 가이드라인 준수

### Core Screens and Views

**1. Settings Panel (설정 패널)**
- 마커 관리 UI (추가/편집/삭제, 중요도 설정)
- 스캔 경로 및 필터 설정
- 이슈 트래커 연동 설정 (GitHub/GitLab 토큰, 저장소 URL)
- 출력 형식 설정

**2. TODO Extraction Tool Window (메인 작업 창)**
- 추출 실행 버튼 및 진행 상태 표시
- 추출 결과 목록 (파일 경로, 라인 번호, 마커, 주석 내용)
- 그룹화/정렬 옵션 (마커별, 파일별, 우선순위별)
- 검색 및 필터 기능

**3. Preview Dialog (미리보기 다이얼로그)**
- 이슈 생성 전 선택 가능한 TODO 목록
- 일괄 선택/해제 체크박스
- 각 항목별 이슈 제목 미리보기
- 실행/취소 버튼

**4. Output Window Integration (출력 창 통합)**
- Visual Studio 출력 창에 "Todo Extractor" 채널 추가
- 상세 로그 및 작업 결과 표시

**5. First-Run Wizard (첫 실행 마법사, 선택적)**
- 마커 기본 설정 안내
- GitHub/GitLab 토큰 설정 가이드
- 간단한 테스트 실행으로 성공 경험 제공

### Accessibility

**등급:** WCAG AA 준수

**주요 요구사항:**
- 키보드만으로 모든 기능 접근 가능
- 스크린 리더 지원 (ARIA 레이블, 명확한 포커스 표시)
- 색상만으로 정보를 전달하지 않음 (아이콘과 텍스트 병행)
- 충분한 색상 대비율 (최소 4.5:1)

### Branding

**스타일:**
- Visual Studio 2022의 네이티브 디자인 언어를 따름
- 확장 프로그램 고유 아이콘 (체크리스트와 화살표를 결합한 심볼)
- 색상: VS 테마 색상을 기본으로 사용하되, 중요도별 구분색 지정
  - 높음: 빨간색 계열
  - 보통: 노란색 계열
  - 낮음: 파란색 계열

**톤:**
- 전문적이고 효율적인 도구
- 불필요한 장식 배제, 기능 중심 디자인

### Target Device and Platforms

**플랫폼:** Desktop Only (Windows)

**해상도 지원:**
- 최소 해상도: 1280x720
- 권장 해상도: 1920x1080 이상
- 4K 디스플레이 DPI 스케일링 지원

**입력 방식:**
- 키보드 + 마우스 (주)
- 터치 스크린 (부차적, 기본 지원)

## Technical Assumptions

### Repository Structure

**Monorepo**

프로젝트는 단일 저장소(Monorepo) 구조를 사용합니다. Visual Studio 확장 프로그램 프로젝트와 단위 테스트 프로젝트를 하나의 리포지토리에서 관리합니다.

**근거:**
- MVP 단계에서는 단일 확장 프로그램만 개발하므로 Monorepo가 관리 오버헤드를 줄임
- Visual Studio 확장 프로그램 표준 구조를 따르며 간단한 구성 유지

### Service Architecture

**Monolith with Plugin Architecture**

확장 프로그램은 모놀리식 구조로 개발하되, 내부적으로 플러그인 아키텍처를 적용하여 향후 확장성을 확보합니다.

**주요 컴포넌트:**
- **Core Engine:** 파일 스캔 및 주석 추출 로직
- **Issue Tracker Plugins:** IIssueTracker 인터페이스를 구현하는 GitHub, GitLab 플러그인
- **File Exporter Plugins:** IFileExporter 인터페이스를 구현하는 MD, CSV, JSON, HTML, DB 플러그인
- **UI Layer:** WPF 기반 도구 창 및 설정 UI

**근거:**
- 단일 확장 프로그램으로 배포하므로 Monolith가 적합
- 인터페이스 기반 설계로 향후 Jira, Azure DevOps 등 추가 이슈 트래커 지원 용이
- 비동기 처리(async/await)를 활용하여 UI 응답성 보장

**기술 스택:**
- **언어:** C# (.NET 6 또는 .NET Framework 4.8)
- **프레임워크:** Visual Studio SDK, WPF
- **API 클라이언트:** Octokit.NET (GitHub), GitLab.NET
- **데이터 저장:** SQLite (로컬 설정 및 DB 출력), JSON (설정 파일)

### Testing Requirements

**Unit + Integration Testing**

**테스트 전략:**
- **단위 테스트:** 핵심 비즈니스 로직(파일 스캔, 마커 추출, 필터링)에 대한 단위 테스트 작성
- **통합 테스트:** GitHub/GitLab API 연동, 파일 I/O, VS SDK 통합에 대한 통합 테스트
- **수동 테스트:** UI/UX 및 Visual Studio 환경 통합은 수동 테스트로 검증

**테스트 프레임워크:**
- **xUnit** 또는 **NUnit** - 단위 테스트 프레임워크
- **Moq** - API 모킹
- **FluentAssertions** - 가독성 높은 어설션

**테스트 커버리지 목표:**
- 핵심 비즈니스 로직: 80% 이상
- API 통합 레이어: 60% 이상

**근거:**
- MVP 단계에서 E2E 자동화 테스트는 ROI가 낮음
- 단위 테스트와 통합 테스트로 안정성 확보하고, UI는 베타 사용자 피드백으로 검증

### Additional Technical Assumptions and Requests

**보안:**
- API 토큰은 Windows Credential Manager에 암호화 저장
- HTTPS 통신만 허용
- 민감 정보 로깅 금지
- GDPR 준수 (사용자 데이터 최소 수집)

**성능 최적화:**
- 파일 스캔 시 병렬 처리 적용 (Parallel.ForEach)
- 대용량 파일은 스트림 기반 읽기로 메모리 효율성 확보
- UI 스레드 블로킹 방지 (비동기 처리)

**Visual Studio 통합:**
- Solution Explorer 컨텍스트 메뉴 통합
- VS 설정 시스템 연동 (Tools > Options)
- Output Window를 통한 로깅
- VS 테마(Light/Dark) 자동 적용

**API Rate Limiting 처리:**
- GitHub/GitLab API 호출 시 rate limit 확인
- 429 오류 발생 시 exponential backoff 재시도 로직
- 대량 이슈 생성 시 배치 처리 및 진행 상태 표시

**확장성 고려사항:**
- 향후 Jira, Azure DevOps 등 추가 이슈 트래커 지원을 위한 인터페이스 설계
- 다국어 지원을 위한 리소스 파일 구조 준비 (영어 우선)
- 설정 마이그레이션을 위한 버전 관리 (JSON schema versioning)

## Epic List

**Epic 1: Foundation & Core Infrastructure**
프로젝트 초기 설정, 개발 환경 구축, Visual Studio 확장 프로그램 기본 구조를 수립하고 간단한 기능을 통해 배포 가능한 첫 번째 버전을 제공합니다.

**Epic 2: Marker Management & File Scanning**
사용자가 커스텀 마커를 관리하고, 프로젝트 파일을 스캔하여 TODO 주석을 추출하는 핵심 기능을 구현합니다. 필터링 및 설정 UI를 포함합니다.

**Epic 3: Issue Tracker Integration**
GitHub 및 GitLab과의 연동을 통해 추출된 TODO를 자동으로 이슈로 생성하는 기능을 구현합니다. 미리보기 UI와 인증 처리를 포함합니다.

**Epic 4: Local File Export & Polish**
추출 결과를 다양한 파일 형식(MD, CSV, JSON, HTML, DB)으로 저장하는 기능을 추가하고, 전체 사용자 경험을 개선합니다.

## Epic 1: Foundation & Core Infrastructure

**Epic Goal:**
Visual Studio 2022 확장 프로그램의 기본 구조를 설정하고, Git 리포지토리, CI/CD 파이프라인, 프로젝트 구조를 수립합니다. 간단한 도구 창을 통해 "Hello World" 수준의 기능을 제공하여 배포 가능한 첫 번째 버전을 완성합니다.

### Story 1.1: 프로젝트 초기 설정 및 개발 환경 구축

As a developer,
I want to set up the initial project structure with Visual Studio SDK,
so that I can start developing the extension with proper tooling and dependencies.

**Acceptance Criteria:**

1. Visual Studio Extension 프로젝트(.vsix)가 생성되어 있어야 한다.
2. .NET 6 또는 .NET Framework 4.8 타겟 프레임워크가 설정되어 있어야 한다.
3. Visual Studio SDK 및 필요한 NuGet 패키지(VS SDK, WPF)가 설치되어 있어야 한다.
4. Git 리포지토리가 초기화되고 .gitignore 파일이 구성되어 있어야 한다.
5. 프로젝트가 로컬에서 F5 디버깅으로 Visual Studio 실험 인스턴스에서 실행되어야 한다.
6. README.md에 프로젝트 개요 및 개발 환경 설정 방법이 문서화되어 있어야 한다.

### Story 1.2: 단위 테스트 프로젝트 설정

As a developer,
I want to set up a unit test project with xUnit and Moq,
so that I can write tests for the core business logic from the beginning.

**Acceptance Criteria:**

1. xUnit 단위 테스트 프로젝트가 솔루션에 추가되어 있어야 한다.
2. Moq, FluentAssertions 패키지가 설치되어 있어야 한다.
3. 샘플 테스트가 작성되어 있고 실행되어야 한다.
4. 테스트 프로젝트가 메인 확장 프로젝트를 참조해야 한다.
5. 테스트 실행 시 코드 커버리지 리포트가 생성되어야 한다(선택적).

### Story 1.3: CI/CD 파이프라인 구축

As a developer,
I want to set up GitHub Actions for automated build and test,
so that code quality is maintained and deployment is automated.

**Acceptance Criteria:**

1. GitHub Actions 워크플로우 파일(.github/workflows/build.yml)이 생성되어 있어야 한다.
2. Push 및 Pull Request 시 자동으로 빌드가 실행되어야 한다.
3. 단위 테스트가 CI에서 자동으로 실행되고 결과가 표시되어야 한다.
4. 빌드 실패 시 GitHub에서 알림을 받아야 한다.
5. 릴리스 태그 생성 시 .vsix 파일이 자동으로 GitHub Release에 업로드되어야 한다(선택적, MVP 후반).

### Story 1.4: 기본 도구 창 UI 생성

As a developer,
I want to create a basic tool window in Visual Studio,
so that users can access the extension's UI.

**Acceptance Criteria:**

1. "Todo Extractor" 도구 창이 Visual Studio의 View > Other Windows 메뉴에 표시되어야 한다.
2. 도구 창을 열면 WPF 기반 UI가 표시되어야 한다.
3. UI에 "Todo Extractor" 제목과 간단한 플레이스홀더 메시지가 표시되어야 한다.
4. VS Light/Dark 테마에 따라 UI 색상이 자동으로 적용되어야 한다.
5. 도구 창은 도킹 가능해야 한다(왼쪽, 오른쪽, 하단).

### Story 1.5: Visual Studio Output Window 통합

As a developer,
I want to integrate with the Visual Studio Output Window,
so that I can provide detailed logs for debugging and user feedback.

**Acceptance Criteria:**

1. Visual Studio Output Window에 "Todo Extractor" 출력 채널이 생성되어야 한다.
2. 확장 프로그램이 로드될 때 "Todo Extractor initialized" 메시지가 출력되어야 한다.
3. 로깅 서비스 클래스(Logger)가 구현되어 Info, Warning, Error 로그를 출력해야 한다.
4. 민감 정보(예: API 토큰)가 로그에 포함되지 않아야 한다.

## Epic 2: Marker Management & File Scanning

**Epic Goal:**
사용자가 커스텀 마커를 정의하고 관리할 수 있는 기능과, 프로젝트 파일을 스캔하여 TODO 주석을 추출하는 핵심 엔진을 구현합니다. 추출 결과를 도구 창에 표시하고, 필터링 및 그룹화 기능을 제공합니다.

### Story 2.1: 마커 설정 데이터 모델 및 저장소 구현

As a developer,
I want to implement a data model and repository for marker settings,
so that users can define and persist custom markers.

**Acceptance Criteria:**

1. Marker 클래스(Name, Priority, CaseSensitive)가 정의되어 있어야 한다.
2. MarkerSettings 클래스가 마커 리스트를 관리해야 한다.
3. 설정이 JSON 형식으로 사용자 로컬 폴더(%AppData%)에 저장되어야 한다.
4. 설정 로드 및 저장 기능이 구현되어 있어야 한다(SettingsRepository).
5. 기본 마커(TODO, FIXME, OPTIMIZE)가 초기 설정에 포함되어 있어야 한다.

### Story 2.2: 마커 관리 UI 구현

As a user,
I want to add, edit, and delete custom markers,
so that I can track different types of tasks in my code.

**Acceptance Criteria:**

1. 설정 패널에 마커 관리 UI가 표시되어야 한다.
2. 마커 목록이 DataGrid 또는 ListView로 표시되어야 한다.
3. "Add", "Edit", "Delete" 버튼이 제공되어야 한다.
4. 마커 추가/편집 시 이름, 중요도(High/Medium/Low), 대소문자 구분 옵션을 설정할 수 있어야 한다.
5. 마커 중복 시 오류 메시지가 표시되어야 한다.
6. 변경 사항이 즉시 저장되어야 한다.

### Story 2.3: 파일 스캔 엔진 구현

As a developer,
I want to implement a file scanning engine,
so that the system can extract TODO comments from project files.

**Acceptance Criteria:**

1. FileScannerService 클래스가 지정된 경로의 파일을 스캔해야 한다.
2. `//마커` 패턴의 주석을 정규식으로 추출해야 한다.
3. 파일 경로, 라인 번호, 마커 타입, 주석 내용이 포함된 TodoItem 객체를 반환해야 한다.
4. 비동기 처리(async/await)를 사용하여 UI 블로킹을 방지해야 한다.
5. 스캔 진행 상황을 IProgress를 통해 보고해야 한다.
6. 단위 테스트로 스캔 로직이 검증되어야 한다(최소 80% 커버리지).

### Story 2.4: 파일 필터 및 제외 경로 설정

As a user,
I want to configure file extension filters and exclude directories,
so that I can scan only relevant files and avoid unnecessary paths.

**Acceptance Criteria:**

1. 설정 UI에 "File Extensions" 입력 필드가 제공되어야 한다(.cs, .cpp 등).
2. "Exclude Directories" 입력 필드가 제공되어야 한다(bin, obj, node_modules 등).
3. 기본값으로 .cs, .cpp, .h, .js, .py가 설정되어 있어야 한다.
4. 기본 제외 경로로 bin, obj, node_modules, .git이 설정되어 있어야 한다.
5. 스캔 엔진이 이 설정을 반영하여 파일을 필터링해야 한다.
6. 설정이 JSON 파일에 저장되어야 한다.

### Story 2.5: 스캔 경로 선택 UI 구현

As a user,
I want to select the scan path (current file, project, or solution),
so that I can control the scope of TODO extraction.

**Acceptance Criteria:**

1. 도구 창에 "Scan Scope" 드롭다운 메뉴가 제공되어야 한다.
2. 옵션: "Current File", "Current Project", "Entire Solution"이 제공되어야 한다.
3. Solution Explorer에서 우클릭 시 "Extract TODOs from [File/Project/Solution]" 컨텍스트 메뉴가 표시되어야 한다.
4. 선택된 범위에 따라 스캔 경로가 결정되어야 한다.
5. 경로 선택 후 "Scan" 버튼 클릭 시 스캔이 시작되어야 한다.

### Story 2.6: 추출 결과 표시 UI 구현

As a user,
I want to see the extracted TODO items in a list,
so that I can review and select items for further actions.

**Acceptance Criteria:**

1. 도구 창에 추출된 TODO 항목이 DataGrid 또는 ListView로 표시되어야 한다.
2. 각 항목은 마커, 파일 경로, 라인 번호, 주석 내용을 포함해야 한다.
3. 항목 더블클릭 시 해당 파일의 라인으로 이동해야 한다.
4. 중요도에 따라 색상 코딩이 적용되어야 한다(High: 빨강, Medium: 노랑, Low: 파랑).
5. 추출 결과가 없을 경우 "No TODOs found" 메시지가 표시되어야 한다.

### Story 2.7: 그룹화 및 정렬 기능 구현

As a user,
I want to group and sort extracted TODOs,
so that I can organize them by marker, file, or priority.

**Acceptance Criteria:**

1. "Group By" 드롭다운 메뉴가 제공되어야 한다(None, Marker, File, Priority).
2. "Sort By" 드롭다운 메뉴가 제공되어야 한다(File Path, Line Number, Marker, Priority).
3. 그룹화 선택 시 UI가 즉시 업데이트되어야 한다.
4. 정렬 선택 시 목록이 즉시 재정렬되어야 한다.
5. 그룹화 및 정렬 설정이 세션 간 유지되어야 한다.

### Story 2.8: 검색 및 필터 기능 구현

As a user,
I want to search and filter TODO items,
so that I can quickly find specific tasks.

**Acceptance Criteria:**

1. 검색 입력 필드가 도구 창 상단에 제공되어야 한다.
2. 검색어 입력 시 실시간으로 결과가 필터링되어야 한다(파일 경로 또는 주석 내용 검색).
3. 마커 타입별 필터 체크박스가 제공되어야 한다(TODO, FIXME 등).
4. 필터 선택 시 해당 마커의 항목만 표시되어야 한다.
5. "Clear Filters" 버튼이 제공되어야 한다.

## Epic 3: Issue Tracker Integration

**Epic Goal:**
GitHub 및 GitLab과의 API 연동을 구현하여 추출된 TODO를 자동으로 이슈로 생성하는 기능을 제공합니다. API 토큰 인증, 미리보기 UI, 배치 처리 및 오류 처리를 포함합니다.

### Story 3.1: 이슈 트래커 플러그인 인터페이스 설계

As a developer,
I want to design an IIssueTracker interface,
so that multiple issue trackers can be supported with a common abstraction.

**Acceptance Criteria:**

1. IIssueTracker 인터페이스가 정의되어 있어야 한다(CreateIssue, ValidateAuth 메서드).
2. TodoItem을 Issue 객체로 변환하는 로직이 구현되어 있어야 한다.
3. 인터페이스가 비동기 메서드를 지원해야 한다(Task<Result>).
4. 오류 처리를 위한 Result 패턴이 구현되어 있어야 한다(Success/Failure).

### Story 3.2: GitHub API 연동 구현

As a user,
I want to create GitHub issues from extracted TODOs,
so that I can track them in my GitHub repository.

**Acceptance Criteria:**

1. GitHubIssueTracker 클래스가 IIssueTracker를 구현해야 한다.
2. Octokit.NET을 사용하여 GitHub API에 연결해야 한다.
3. Personal Access Token으로 인증해야 한다.
4. 이슈 제목은 주석 내용, 본문은 파일 경로와 라인 번호를 포함해야 한다.
5. 기본 라벨(예: "todo-extractor", "automated")이 자동으로 추가되어야 한다.
6. API rate limit 확인 로직이 포함되어야 한다.
7. 단위 테스트로 로직이 검증되어야 한다(Moq으로 API 모킹).

### Story 3.3: GitLab API 연동 구현

As a user,
I want to create GitLab issues from extracted TODOs,
so that I can track them in my GitLab repository.

**Acceptance Criteria:**

1. GitLabIssueTracker 클래스가 IIssueTracker를 구현해야 한다.
2. GitLab.NET 또는 HttpClient를 사용하여 GitLab API에 연결해야 한다.
3. Personal Access Token으로 인증해야 한다.
4. 이슈 제목은 주석 내용, 본문은 파일 경로와 라인 번호를 포함해야 한다.
5. 기본 라벨이 자동으로 추가되어야 한다.
6. API rate limit 확인 로직이 포함되어야 한다.
7. 단위 테스트로 로직이 검증되어야 한다.

### Story 3.4: API 토큰 저장 및 관리

As a user,
I want to securely store my GitHub/GitLab API tokens,
so that I don't have to enter them every time.

**Acceptance Criteria:**

1. Windows Credential Manager를 사용하여 API 토큰을 암호화 저장해야 한다.
2. 설정 UI에 GitHub 및 GitLab 토큰 입력 필드가 제공되어야 한다.
3. 토큰이 마스킹되어 표시되어야 한다(예: ****).
4. 토큰이 평문으로 JSON 파일에 저장되지 않아야 한다.
5. 토큰 변경 시 즉시 Credential Manager에 업데이트되어야 한다.
6. 토큰 검증 버튼("Test Connection")이 제공되어야 한다.

### Story 3.5: 이슈 트래커 설정 UI 구현

As a user,
I want to configure my issue tracker settings (repository URL, labels, assignee),
so that issues are created correctly.

**Acceptance Criteria:**

1. 설정 패널에 "Issue Tracker" 섹션이 제공되어야 한다.
2. GitHub/GitLab 선택 라디오 버튼이 제공되어야 한다.
3. 저장소 URL 입력 필드가 제공되어야 한다(예: owner/repo).
4. 기본 라벨 입력 필드가 제공되어야 한다(쉼표로 구분).
5. 기본 담당자 입력 필드가 제공되어야 한다(선택적).
6. 설정이 JSON 파일에 저장되어야 한다.
7. "Test Connection" 버튼 클릭 시 API 연결이 검증되어야 한다.

### Story 3.6: 이슈 생성 미리보기 UI 구현

As a user,
I want to preview and select TODOs before creating issues,
so that I can avoid creating unwanted issues.

**Acceptance Criteria:**

1. "Create Issues" 버튼 클릭 시 미리보기 다이얼로그가 표시되어야 한다.
2. 추출된 TODO 목록이 체크박스와 함께 표시되어야 한다.
3. 각 항목의 이슈 제목 미리보기가 표시되어야 한다.
4. "Select All" / "Deselect All" 버튼이 제공되어야 한다.
5. "Create" 버튼 클릭 시 선택된 항목만 이슈로 생성되어야 한다.
6. "Cancel" 버튼 클릭 시 다이얼로그가 닫혀야 한다.

### Story 3.7: 배치 이슈 생성 및 진행 상태 표시

As a user,
I want to see the progress of issue creation,
so that I know how many issues have been created and if any failed.

**Acceptance Criteria:**

1. 이슈 생성 중 프로그레스 바가 표시되어야 한다.
2. "Creating issue 3 of 10..." 형식의 상태 메시지가 표시되어야 한다.
3. 성공 및 실패 건수가 표시되어야 한다.
4. 실패한 항목에 대한 오류 메시지가 Output Window에 로그되어야 한다.
5. API rate limit 도달 시 exponential backoff 재시도가 적용되어야 한다.
6. 작업 완료 시 "10 issues created, 2 failed" 형식의 요약이 표시되어야 한다.
7. 사용자가 작업을 중간에 취소할 수 있어야 한다("Cancel" 버튼).

### Story 3.8: 이슈 중복 방지 메커니즘 구현 (선택적, Post-MVP)

As a user,
I want the system to detect if a TODO has already been converted to an issue,
so that I don't create duplicate issues.

**Acceptance Criteria:**

1. 이슈 생성 시 이슈 제목 및 본문 해시를 로컬에 저장해야 한다.
2. 동일한 TODO를 다시 추출 시 "Already created" 표시가 되어야 한다.
3. 미리보기 UI에서 중복 항목이 회색으로 표시되고 체크박스가 비활성화되어야 한다.
4. 사용자가 "Force recreate" 옵션을 선택할 수 있어야 한다.

## Epic 4: Local File Export & Polish

**Epic Goal:**
추출된 TODO를 다양한 파일 형식(MD, CSV, JSON, HTML, SQLite)으로 저장하는 기능을 구현하고, 전체 사용자 경험을 개선합니다. 첫 실행 마법사, 키보드 단축키, 최종 UI 개선을 포함합니다.

### Story 4.1: 파일 내보내기 플러그인 인터페이스 설계

As a developer,
I want to design an IFileExporter interface,
so that multiple file formats can be supported with a common abstraction.

**Acceptance Criteria:**

1. IFileExporter 인터페이스가 정의되어 있어야 한다(Export 메서드).
2. 인터페이스가 파일 경로, TODO 항목 리스트, 그룹화 옵션을 매개변수로 받아야 한다.
3. 비동기 메서드를 지원해야 한다(Task<Result>).

### Story 4.2: Markdown 내보내기 구현

As a user,
I want to export extracted TODOs to a Markdown file,
so that I can review them in a readable format.

**Acceptance Criteria:**

1. MarkdownExporter 클래스가 IFileExporter를 구현해야 한다.
2. 그룹화 옵션(마커별/파일별/우선순위별)에 따라 Markdown 헤더가 생성되어야 한다.
3. 각 TODO 항목이 마크다운 리스트 형식으로 출력되어야 한다.
4. 파일 경로가 링크로 변환되어야 한다(선택적).
5. 생성된 파일이 사용자가 지정한 경로에 저장되어야 한다.
6. 단위 테스트로 출력 형식이 검증되어야 한다.

### Story 4.3: CSV 내보내기 구현

As a user,
I want to export extracted TODOs to a CSV file,
so that I can analyze them in Excel or other tools.

**Acceptance Criteria:**

1. CsvExporter 클래스가 IFileExporter를 구현해야 한다.
2. CSV 헤더: Marker, File Path, Line Number, Comment, Priority가 포함되어야 한다.
3. 각 TODO 항목이 한 행으로 출력되어야 한다.
4. 쉼표가 포함된 주석 내용은 따옴표로 이스케이프되어야 한다.
5. 생성된 파일이 사용자가 지정한 경로에 저장되어야 한다.
6. 단위 테스트로 출력 형식이 검증되어야 한다.

### Story 4.4: JSON 내보내기 구현

As a user,
I want to export extracted TODOs to a JSON file,
so that I can process them programmatically.

**Acceptance Criteria:**

1. JsonExporter 클래스가 IFileExporter를 구현해야 한다.
2. JSON 구조: { "todos": [ { "marker": "TODO", "filePath": "...", "lineNumber": 10, "comment": "...", "priority": "High" } ] }
3. JSON이 올바른 형식으로 직렬화되어야 한다(System.Text.Json 또는 Newtonsoft.Json).
4. 생성된 파일이 사용자가 지정한 경로에 저장되어야 한다.
5. 단위 테스트로 출력 형식이 검증되어야 한다.

### Story 4.5: HTML 내보내기 구현

As a user,
I want to export extracted TODOs to an HTML file,
so that I can view them in a web browser with styling.

**Acceptance Criteria:**

1. HtmlExporter 클래스가 IFileExporter를 구현해야 한다.
2. HTML 테이블 형식으로 TODO 항목이 출력되어야 한다.
3. 중요도에 따라 행 색상이 적용되어야 한다(High: 빨강, Medium: 노랑, Low: 파랑).
4. 그룹화 옵션에 따라 섹션 헤더가 추가되어야 한다.
5. 생성된 파일이 브라우저에서 올바르게 렌더링되어야 한다.
6. 단위 테스트로 출력 형식이 검증되어야 한다.

### Story 4.6: SQLite 데이터베이스 내보내기 구현

As a user,
I want to export extracted TODOs to a SQLite database,
so that I can query and analyze them with SQL.

**Acceptance Criteria:**

1. DatabaseExporter 클래스가 IFileExporter를 구현해야 한다.
2. SQLite 데이터베이스 파일(.db)이 생성되어야 한다.
3. "todos" 테이블이 생성되어야 한다(id, marker, file_path, line_number, comment, priority, created_at).
4. 각 TODO 항목이 테이블에 삽입되어야 한다.
5. 데이터베이스가 SQLite 클라이언트로 열리고 쿼리 가능해야 한다.
6. 단위 테스트로 데이터베이스 생성 및 삽입이 검증되어야 한다.

### Story 4.7: 파일 내보내기 UI 구현

As a user,
I want to configure and trigger file export,
so that I can save extracted TODOs in my preferred format.

**Acceptance Criteria:**

1. "Export" 버튼이 도구 창에 제공되어야 한다.
2. 버튼 클릭 시 파일 형식 선택 다이얼로그가 표시되어야 한다(MD, CSV, JSON, HTML, DB).
3. 파일 저장 경로를 선택할 수 있는 SaveFileDialog가 표시되어야 한다.
4. 그룹화 옵션(마커별/파일별/우선순위별) 선택 UI가 제공되어야 한다.
5. "Export" 실행 후 성공/실패 메시지가 표시되어야 한다.
6. Output Window에 상세 로그가 출력되어야 한다.

### Story 4.8: 키보드 단축키 구현

As a user,
I want to use keyboard shortcuts for common actions,
so that I can work more efficiently.

**Acceptance Criteria:**

1. Ctrl+Shift+T: TODO 추출 실행
2. Ctrl+Shift+E: 내보내기 다이얼로그 열기
3. F5 또는 Ctrl+R: 스캔 새로고침
4. Esc: 다이얼로그 닫기
5. 키보드 단축키가 Visual Studio의 키 바인딩 시스템에 등록되어야 한다.
6. 설정 UI에 키보드 단축키 안내가 표시되어야 한다.

### Story 4.9: 첫 실행 마법사 구현 (선택적)

As a new user,
I want to be guided through initial setup,
so that I can quickly start using the extension.

**Acceptance Criteria:**

1. 확장 프로그램 첫 실행 시 마법사 다이얼로그가 표시되어야 한다.
2. 단계 1: 기본 마커 설정 안내 및 확인
3. 단계 2: GitHub/GitLab API 토큰 설정 안내 (선택적 건너뛰기 가능)
4. 단계 3: 간단한 테스트 스캔 실행
5. 마법사 완료 후 "Don't show again" 옵션이 제공되어야 한다.
6. 설정에서 마법사를 다시 실행할 수 있어야 한다.

### Story 4.10: UI/UX 최종 개선 및 폴리시

As a user,
I want a polished and professional user interface,
so that the extension feels complete and reliable.

**Acceptance Criteria:**

1. 모든 버튼과 입력 필드에 툴팁이 제공되어야 한다.
2. 아이콘이 모든 버튼과 메뉴 항목에 추가되어야 한다.
3. 로딩 중 스피너 애니메이션이 표시되어야 한다.
4. 오류 메시지가 사용자 친화적이고 명확해야 한다.
5. VS Light/Dark 테마에서 모든 UI 요소가 올바르게 표시되어야 한다.
6. 접근성(Accessibility) 요구사항이 충족되어야 한다(WCAG AA).
7. 베타 테스터 피드백을 반영한 개선사항이 적용되어야 한다.

## Checklist Results Report

이 PRD는 PM 체크리스트를 기반으로 검토되었습니다.

**주요 검토 항목:**

✅ **명확성 (Clarity)**
- 모든 요구사항이 명확하고 모호하지 않게 작성되었습니다.
- 각 User Story는 "As a [role], I want [feature], so that [benefit]" 형식을 따릅니다.
- Acceptance Criteria는 구체적이고 검증 가능합니다.

✅ **완전성 (Completeness)**
- Functional Requirements (FR1-FR15) 및 Non-Functional Requirements (NFR1-NFR12)가 모두 정의되었습니다.
- 4개의 Epic과 총 32개의 User Story가 작성되었습니다.
- UI/UX, 기술적 가정, Epic 세부사항이 모두 포함되었습니다.

✅ **추적 가능성 (Traceability)**
- 모든 Requirements가 브리프 문서의 MVP Scope와 연결됩니다.
- 각 Epic과 Story는 Goals 섹션의 비즈니스 목표를 지원합니다.

✅ **실행 가능성 (Feasibility)**
- 기술 스택(C#, WPF, VS SDK)이 명확히 정의되었습니다.
- MVP 범위가 3-4개월 개발 일정에 적합합니다.
- 각 Story는 AI 에이전트가 독립적으로 완료할 수 있는 크기입니다.

✅ **우선순위 (Prioritization)**
- Epic 순서가 논리적으로 배치되었습니다 (Foundation → Core Features → Integration → Polish).
- Story 순서가 의존성을 고려하여 배치되었습니다.

⚠️ **개선 권장사항:**
- Epic 3.8 (이슈 중복 방지)은 Post-MVP로 표시되어 있으나, 실제 구현 우선순위는 사용자 피드백에 따라 조정 필요.
- Story 4.9 (첫 실행 마법사)는 선택적으로 표시되어 있으나, NFR9 ("첫 실행 후 5분 내 첫 이슈 생성 80% 달성")를 위해 MVP에 포함 검토 필요.

## Next Steps

### UX Expert Prompt

UX Expert 에이전트에게 전달할 프롬프트:

```
이 Todo Extractor PRD를 검토하고, UI/UX 디자인 아키텍처를 작성해주세요.

주요 작업:
1. PRD의 "User Interface Design Goals" 섹션을 바탕으로 상세 UI 명세 작성
2. 각 Core Screen (Settings Panel, Tool Window, Preview Dialog)에 대한 와이어프레임 또는 컴포넌트 구조 정의
3. WPF XAML 기반 UI 아키텍처 설계 (MVVM 패턴 적용)
4. Visual Studio 테마 통합 및 접근성(WCAG AA) 구현 가이드라인
5. 사용자 여정(User Journey) 맵핑: 첫 실행 → 설정 → 스캔 → 이슈 생성

참고 문서:
- docs/prd.md (이 문서)
- docs/brief.md (프로젝트 브리프)

목표: "첫 실행 후 5분 내 첫 이슈 생성" 달성을 위한 직관적 UX 설계
```

### Architect Prompt

Architect 에이전트에게 전달할 프롬프트:

```
이 Todo Extractor PRD를 검토하고, 기술 아키텍처 문서를 작성해주세요.

주요 작업:
1. PRD의 "Technical Assumptions" 섹션을 바탕으로 상세 시스템 아키텍처 설계
2. 프로젝트 구조 정의 (폴더 구조, 네임스페이스, 주요 클래스 및 인터페이스)
3. 플러그인 아키텍처 상세 설계 (IIssueTracker, IFileExporter 인터페이스 및 구현체)
4. 데이터 흐름 다이어그램 (파일 스캔 → TODO 추출 → 이슈 생성 / 파일 내보내기)
5. 성능 최적화 전략 (비동기 처리, 병렬 스캔, 메모리 관리)
6. 보안 설계 (API 토큰 저장, HTTPS 통신, 로깅 정책)
7. 테스트 전략 상세화 (단위 테스트, 통합 테스트, 모킹 전략)
8. CI/CD 파이프라인 구성

기술 스택:
- C# (.NET 6 또는 .NET Framework 4.8)
- Visual Studio SDK, WPF (MVVM)
- Octokit.NET (GitHub), GitLab.NET
- SQLite, JSON
- xUnit, Moq, FluentAssertions

참고 문서:
- docs/prd.md (이 문서)
- docs/brief.md (프로젝트 브리프)

목표: 확장 가능하고 유지보수 가능한 Visual Studio 확장 프로그램 아키텍처 설계
```

---

**PRD 작성 완료**

이 PRD는 Todo Extractor 프로젝트의 MVP 개발을 위한 완전한 요구사항 문서입니다. 다음 단계는 UX Expert와 Architect 에이전트와 협업하여 설계 문서를 작성하고, Dev 에이전트와 함께 Epic 1부터 순차적으로 개발을 시작하는 것입니다.

