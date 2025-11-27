# Epic 3: Issue Tracker Integration

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
