# Epic 2: Marker Management & File Scanning

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
