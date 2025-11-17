# 📝 Step 2: Notion 데이터베이스 설정

이 문서는 Notion에서 데이터베이스를 생성하고 설정하는 방법을 설명합니다.

## 목차
- [Notion Integration 생성](#notion-integration-생성)
- [데이터베이스 생성](#데이터베이스-생성)
- [필수 속성 설정](#필수-속성-설정)
- [Projects 필드 추가](#projects-필드-추가)
- [Integration 연결](#integration-연결)
- [확인 사항](#확인-사항)

---

## Notion Integration 생성

### 1. Notion Integrations 페이지 접속

브라우저에서 다음 URL로 이동:
```
https://www.notion.so/my-integrations
```

또는:
1. Notion 앱/웹 열기
2. 좌측 하단 **"Settings & members"** 클릭
3. **"Integrations"** 클릭
4. **"Develop your own integrations"** 클릭

### 2. New Integration 생성

- **"+ New integration"** 버튼 클릭

### 3. Integration 정보 입력

```
Name: GitHub Issue Sync
   (또는 원하는 이름)

Associated workspace: [본인의 워크스페이스 선택]

Type: Internal (기본값)
```

### 4. Capabilities 설정

다음 권한을 **활성화**:

- ✅ **Read content**
- ✅ **Update content**
- ✅ **Insert content**

**Comment capabilities**는 비활성화해도 됩니다 (선택사항).

### 5. Submit 클릭

**"Submit"** 버튼 클릭하여 Integration 생성!

### 6. API Key 복사

생성 완료 후 **"Internal Integration Token"**이 표시됩니다.

```
Token 형식:
secret_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

⚠️ **중요**: 이 Token을 안전한 곳에 복사해두세요! 
- 이 페이지를 벗어나면 다시 볼 수 없습니다
- 만약 잃어버렸다면 "Show token" → "Regenerate token" 하면 됩니다

---

## 데이터베이스 생성

### 1. Notion 페이지 만들기

1. Notion에서 **"+ New page"** 클릭
2. 페이지 제목 입력: `GitHub Issues` (또는 원하는 이름)

### 2. 데이터베이스 추가

1. 페이지 내에서 `/database` 입력
2. **"Database - Inline"** 선택
3. 데이터베이스 이름: `Issues Database`

또는:

1. 페이지 내에서 **"Table"** 블록 추가
2. **"Database"** 옵션 선택

---

## 필수 속성 설정

Notion 데이터베이스는 기본적으로 **Name** 속성이 있습니다. 이를 **Title**로 사용합니다.

### 기본 속성

| 속성 이름 | 타입 | 설명 |
|-----------|------|------|
| **Title** | Title | 이슈 제목 (Name을 그대로 사용) |

### 추가할 필수 속성

데이터베이스의 **"+ New property"** 또는 열 헤더의 **"+"** 버튼을 클릭하여 추가:

#### 1️⃣ Issue Number
```
Property name: Issue Number
Property type: Number
Format: Number (0 decimals)
```

**추가 방법:**
1. 열 헤더 우측 **"+"** 클릭
2. **"Number"** 선택
3. 속성 이름: `Issue Number`
4. Number format: **Number**

#### 2️⃣ Status
```
Property name: Status
Property type: Select
Options:
  - Open
  - Closed
```

**추가 방법:**
1. **"+"** → **"Select"** 선택
2. 속성 이름: `Status`
3. Options 추가:
   - `Open` (🟢 초록색)
   - `Closed` (⚫ 회색)

#### 3️⃣ Labels
```
Property name: Labels
Property type: Text
```

**추가 방법:**
1. **"+"** → **"Text"** 선택
2. 속성 이름: `Labels`

#### 4️⃣ URL
```
Property name: URL
Property type: URL
```

**추가 방법:**
1. **"+"** → **"URL"** 선택
2. 속성 이름: `URL`

#### 5️⃣ Created At
```
Property name: Created At
Property type: Date
```

**추가 방법:**
1. **"+"** → **"Date"** 선택
2. 속성 이름: `Created At`

#### 6️⃣ Assignee
```
Property name: Assignee
Property type: Text
```

**추가 방법:**
1. **"+"** → **"Text"** 선택
2. 속성 이름: `Assignee`

---

## Projects 필드 추가

GitHub Projects V2의 정보를 저장할 속성들을 추가합니다.

#### 7️⃣ Repository
```
Property name: Repository
Property type: Text
설명: 어느 레포의 이슈인지 (나중에 여러 레포 동기화 시 필요)
```

#### 8️⃣ Project
```
Property name: Project
Property type: Text
설명: 프로젝트 이름
```

#### 9️⃣ Project Status
```
Property name: Project Status
Property type: Select
Options:
  - Backlog
  - StandBy
  - Todo
  - In Progress
  - Done
```

**추가 방법:**
1. **"+"** → **"Select"** 선택
2. 속성 이름: `Project Status`
3. Options 추가 (GitHub Projects의 Status와 동일하게):
   - `Backlog`
   - `StandBy`
   - `Todo`
   - `In Progress`
   - `Done`

#### 🔟 Priority
```
Property name: Priority
Property type: Select
Options:
  - Critical
  - High
  - Medium
  - Low
```

**추가 방법:**
1. **"+"** → **"Select"** 선택
2. 속성 이름: `Priority`
3. Options 추가:
   - `Critical` (🔴)
   - `High` (🟠)
   - `Medium` (🟡)
   - `Low` (🟢)

#### 1️⃣1️⃣ Story Points
```
Property name: Story Points
Property type: Number
```

#### 1️⃣2️⃣ Capacity
```
Property name: Capacity
Property type: Number
```

#### 1️⃣3️⃣ Sprint (선택사항)
```
Property name: Sprint
Property type: Text
```

---

## 최종 데이터베이스 구조

설정을 완료하면 다음과 같은 속성들이 있어야 합니다:

| # | 속성 이름 | 타입 | 필수 | 설명 |
|---|-----------|------|------|------|
| 1 | **Title** | Title | ✅ | 이슈 제목 |
| 2 | **Issue Number** | Number | ✅ | 이슈 번호 |
| 3 | **Status** | Select | ✅ | Open/Closed |
| 4 | **Labels** | Text | ✅ | 라벨 목록 |
| 5 | **URL** | URL | ✅ | GitHub 링크 |
| 6 | **Created At** | Date | ✅ | 생성일 |
| 7 | **Assignee** | Text | ⭕ | 담당자 |
| 8 | **Repository** | Text | ⭕ | 레포 이름 |
| 9 | **Project** | Text | 🎯 | 프로젝트 이름 |
| 10 | **Project Status** | Select | 🎯 | 프로젝트 상태 |
| 11 | **Priority** | Select | 🎯 | 우선순위 |
| 12 | **Story Points** | Number | 🎯 | 스토리 포인트 |
| 13 | **Capacity** | Number | 🎯 | 용량 |
| 14 | **Sprint** | Text | ⭕ | 스프린트 |

**범례:**
- ✅ 필수: 기본 동기화에 필요
- 🎯 Projects: Projects V2 연동 시 필요
- ⭕ 선택: 있으면 좋음

---

## Integration 연결

데이터베이스를 생성했으면 Integration과 연결해야 합니다!

### 1. 데이터베이스 페이지에서 연결

1. 데이터베이스가 있는 페이지로 이동
2. 우측 상단 **"..."** (점 3개) 메뉴 클릭
3. **"Connections"** 또는 **"Add connections"** 클릭
4. 방금 만든 Integration 선택: `GitHub Issue Sync`
5. **"Confirm"** 클릭

### 2. 연결 확인

Integration이 연결되면:
```
✅ GitHub Issue Sync has access to this page
```

---

## Database ID 확인

동기화 스크립트가 데이터베이스를 찾으려면 **Database ID**가 필요합니다.

### 방법 1: URL에서 추출 (추천)

데이터베이스를 Full page로 열었을 때 URL:
```
https://www.notion.so/{workspace}/{DATABASE_ID}?v={view_id}

예시:
https://www.notion.so/myworkspace/a1b2c3d4e5f67890a1b2c3d4e5f67890?v=...
                                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                 이 부분이 Database ID
```

**Database ID:**
- 32자리 영숫자 문자열
- 하이픈(-) 없음
- 예: `a1b2c3d4e5f67890a1b2c3d4e5f67890`

### 방법 2: Share 링크에서 추출

1. 데이터베이스 우측 상단 **"Share"** 클릭
2. **"Copy link"** 클릭
3. URL에서 32자리 ID 추출

### 하이픈 제거

URL에 하이픈이 있다면 제거하세요:
```
a1b2c3d4-e5f6-7890-a1b2-c3d4e5f67890
↓ (하이픈 제거)
a1b2c3d4e5f67890a1b2c3d4e5f67890
```

⚠️ **중요**: Database ID를 메모해두세요!

---

## 확인 사항

### ✅ 체크리스트

설정을 완료했다면 다음을 확인하세요:

- [ ] Notion Integration 생성 완료
- [ ] **API Key 복사** 완료 (안전하게 보관)
- [ ] Notion 데이터베이스 생성 완료
- [ ] 필수 속성 추가 완료:
  - [ ] Title (기본 제공)
  - [ ] Issue Number
  - [ ] Status (Open/Closed)
  - [ ] Labels
  - [ ] URL
  - [ ] Created At
  - [ ] Assignee
- [ ] Projects 속성 추가 완료:
  - [ ] Repository
  - [ ] Project
  - [ ] Project Status
  - [ ] Priority
  - [ ] Story Points
  - [ ] Capacity
- [ ] Integration과 데이터베이스 연결 완료
- [ ] **Database ID 복사** 완료

### 📋 메모해둬야 할 것들

```
✏️ Notion API Key:
secret_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

✏️ Notion Database ID:
a1b2c3d4e5f67890a1b2c3d4e5f67890
```

---

## 다음 단계

✅ Notion 설정 완료!

➡️ **다음**: [Step 3: GitHub Secrets 설정](./03-github-secrets.md)

---

## 문제 해결

### Q: Integration Token을 잃어버렸어요
**A:** 
1. https://www.notion.so/my-integrations 접속
2. Integration 클릭
3. "Show token" → "Regenerate token"

### Q: Database ID를 찾을 수 없어요
**A:** 
1. 데이터베이스를 Full page로 열기 (우측 상단 ⤢ 아이콘)
2. URL 복사
3. 32자리 영숫자 찾기

### Q: Integration 연결이 안 돼요
**A:** 
1. 페이지 최상위에서 연결 시도
2. 데이터베이스가 페이지 내부에 있다면, 상위 페이지에서 Integration 연결
3. "Connections" 메뉴가 안 보이면 "..." → "Add connections"

### Q: 속성 타입을 잘못 선택했어요
**A:** 
1. 속성 헤더 클릭
2. "Edit property" → "Type" 변경
3. 또는 속성 삭제 후 다시 추가

### Q: 동기화 시 "Could not find database" 에러
**A:**
1. Database ID가 정확한지 확인
2. Integration이 연결되었는지 확인
3. Database ID에서 하이픈(-) 제거했는지 확인

