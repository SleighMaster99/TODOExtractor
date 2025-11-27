# Next Steps

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

