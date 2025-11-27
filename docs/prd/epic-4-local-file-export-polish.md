# Epic 4: Local File Export & Polish

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
