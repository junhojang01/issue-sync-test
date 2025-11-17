# 📋 Step 1: GitHub Projects V2 설정

이 문서는 GitHub Projects V2를 생성하고 설정하는 방법을 설명합니다.

## 목차
- [Projects V2 생성](#projects-v2-생성)
- [필드 설정](#필드-설정)
- [이슈 추가](#이슈-추가)
- [확인 사항](#확인-사항)

---

## Projects V2 생성

### 1. GitHub 레포지토리로 이동

브라우저에서 자신의 레포지토리로 이동합니다.
```
예: https://github.com/YOUR_USERNAME/YOUR_REPO
```

### 2. Projects 탭 클릭

상단 메뉴에서 **Projects** 탭을 클릭합니다.

```
Code  Issues  Pull requests  Actions  Projects  Wiki  Security
                                        ^^^^^^^^
```

### 3. 새 Project 생성

- **"Link a project"** 드롭다운 클릭
- **"New project"** 선택
- 새 창/탭이 열립니다

### 4. 템플릿 선택

다음 중 하나를 선택:

| 템플릿 | 설명 | 추천 |
|--------|------|------|
| **Board** | 간판 스타일 (Kanban) | ⭐ 추천 |
| **Table** | 스프레드시트 스타일 | |
| **Roadmap** | 타임라인 뷰 | |
| **Start from scratch** | 빈 프로젝트 | |

**Board** 템플릿으로 시작하는 것을 추천합니다.

### 5. Project 이름 입력

```
예시:
- "2024 Development"
- "Product Roadmap"
- "Issue Tracker"
- "Test Project"
```

### 6. 생성 완료

**"Create project"** 버튼 클릭!

---

## 필드 설정

기본적으로 제공되는 필드:
- **Title** (제목)
- **Status** (상태)
- **Assignees** (담당자)

### 추가 필드 생성 방법

#### 1. 필드 추가 버튼 클릭

프로젝트 화면 우측 상단의 **"+"** 버튼 클릭

```
┌──────────────────────────────────┐
│  Project Name          [+] [...]  │  ← 여기
└──────────────────────────────────┘
```

#### 2. "New field" 선택

드롭다운에서 **"New field"** 클릭

#### 3. 필드 타입 선택

| 필드 타입 | 용도 | 예시 |
|-----------|------|------|
| **Single select** | 하나만 선택 (드롭다운) | Priority: Low/Medium/High |
| **Number** | 숫자 | Capacity, Story Points |
| **Date** | 날짜 | Due Date, Start Date |
| **Text** | 텍스트 | Notes, Description |
| **Iteration** | 스프린트/반복 주기 | Sprint 1, Sprint 2 |

---

## 권장 필드 구성 (테스트용)

### 기본 필드 (이미 있음)
- ✅ **Title** (자동)
- ✅ **Status** (자동) - Todo, In Progress, Done

### 추가 권장 필드

#### 1️⃣ Priority (우선순위)
```
타입: Single select
옵션:
  - 🔴 Critical
  - 🟠 High
  - 🟡 Medium
  - 🟢 Low
```

**설정 방법:**
1. **"+ New field"** 클릭
2. Field name: `Priority`
3. Field type: **Single select**
4. Options 추가:
   - Critical
   - High
   - Medium
   - Low
5. **"Save"** 클릭

#### 2️⃣ Story Points (스토리 포인트)
```
타입: Number
설명: 작업의 복잡도/크기
```

**설정 방법:**
1. **"+ New field"** 클릭
2. Field name: `Story Points`
3. Field type: **Number**
4. **"Save"** 클릭

#### 3️⃣ Capacity (용량)
```
타입: Number
설명: 작업 용량 또는 예상 시간
```

**설정 방법:**
1. **"+ New field"** 클릭
2. Field name: `Capacity`
3. Field type: **Number**
4. **"Save"** 클릭

#### 4️⃣ Sprint (스프린트)
```
타입: Iteration
설명: 스프린트 주기
```

**설정 방법:**
1. **"+ New field"** 클릭
2. Field name: `Sprint`
3. Field type: **Iteration**
4. Duration: `1 week` 또는 `2 weeks`
5. Start date: 오늘 날짜
6. **"Save"** 클릭

---

## Status 필드 커스터마이징 (선택사항)

기본 Status는 `Todo`, `In Progress`, `Done`입니다.

**Backlog, StandBy 등을 추가하려면:**

1. Status 필드의 **"..."** 메뉴 클릭
2. **"Edit field"** 선택
3. 옵션 추가:
   - Backlog
   - StandBy
   - Todo (이미 있음)
   - In Progress (이미 있음)
   - Done (이미 있음)
4. **"Save changes"** 클릭

**최종 Status 옵션 예시:**
```
┌──────────────┐
│ ⚪ Backlog    │
│ 🟡 StandBy   │
│ 🔵 Todo      │
│ 🟣 In Progress│
│ 🟢 Done      │
└──────────────┘
```

---

## 이슈 추가

### 방법 1: 기존 이슈 추가

1. 프로젝트 화면에서 **"Add item"** 클릭
2. **"+ Add item from repository"** 선택
3. 레포지토리의 기존 이슈 검색
4. 추가할 이슈 선택

### 방법 2: 새 이슈 생성

1. 프로젝트 화면에서 **"+ Add item"** 클릭
2. 제목 입력 후 Enter
3. 생성된 draft를 클릭
4. **"Convert to issue"** 선택
5. 레포지토리 선택 후 생성

### 테스트용 이슈 만들기

#### 이슈 #1: 로그인 버그 수정
```
제목: 로그인 버그 수정
본문:
# 문제 설명
로그인 시 타임아웃 발생

## 재현 방법
1. 로그인 페이지 접속
2. 아이디/비밀번호 입력
3. 로그인 버튼 클릭

## 예상 결과
로그인 성공

## 실제 결과
타임아웃 에러

라벨: bug
```

**프로젝트 필드 설정:**
- Status: `In Progress`
- Priority: `High`
- Story Points: `5`
- Capacity: `8`

#### 이슈 #2: 다크모드 추가
```
제목: 다크모드 UI 구현
본문:
# 기능 설명
다크모드 테마 추가

## 체크리스트
- [ ] 디자인 시스템 정의
- [ ] 컴포넌트 수정
- [ ] 토글 버튼 추가
- [ ] 설정 저장 기능

라벨: enhancement
```

**프로젝트 필드 설정:**
- Status: `Todo`
- Priority: `Medium`
- Story Points: `8`
- Capacity: `13`

---

## 확인 사항

### ✅ 체크리스트

설정을 완료했다면 다음을 확인하세요:

- [ ] Projects V2 생성 완료
- [ ] 필드 추가 완료:
  - [ ] Status (기본 제공)
  - [ ] Priority
  - [ ] Story Points
  - [ ] Capacity
  - [ ] Sprint (선택사항)
- [ ] 테스트 이슈 2-3개 생성
- [ ] 이슈를 프로젝트에 추가
- [ ] 각 이슈의 필드 값 설정 (Status, Priority 등)

### 🔍 프로젝트 URL 확인

프로젝트 URL을 메모해두세요. 나중에 필요합니다.

```
형식: https://github.com/users/YOUR_USERNAME/projects/PROJECT_NUMBER
예시: https://github.com/users/jangjunho/projects/1
```

---

## 다음 단계

✅ Projects 설정 완료!

➡️ **다음**: [Step 2: Notion 데이터베이스 설정](./02-notion-setup.md)

---

## 문제 해결

### Q: Projects 탭이 안 보여요
**A:** 레포지토리 Settings → Features에서 Projects를 활성화하세요.

### Q: New Project 버튼이 없어요
**A:** 새 Projects V2가 아닌 Projects (Classic)을 보고 있을 수 있습니다. 
GitHub 좌측 메뉴에서 자신의 프로필 → Projects로 가서 새로 만드세요.

### Q: 필드가 저장이 안 돼요
**A:** 브라우저를 새로고침하고 다시 시도하세요. 또는 다른 브라우저로 시도하세요.

### Q: Iteration 필드가 이상해요
**A:** Iteration은 자동으로 날짜 범위를 생성합니다. Duration을 1-2주로 설정하고 저장하세요.

