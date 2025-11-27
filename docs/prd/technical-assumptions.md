# Technical Assumptions

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
