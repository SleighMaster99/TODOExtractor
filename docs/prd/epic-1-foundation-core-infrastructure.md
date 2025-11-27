# Epic 1: Foundation & Core Infrastructure

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
