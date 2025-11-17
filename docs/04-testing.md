# 🧪 Step 4: 테스트 및 실행

이 문서는 동기화를 테스트하고 확인하는 방법을 설명합니다.

## 목차
- [사전 준비 확인](#사전-준비-확인)
- [첫 번째 동기화 실행](#첫-번째-동기화-실행)
- [결과 확인](#결과-확인)
- [자동 동기화 확인](#자동-동기화-확인)
- [문제 해결](#문제-해결)

---

## 사전 준비 확인

테스트하기 전에 모든 설정이 완료되었는지 확인합니다.

### ✅ 체크리스트

#### GitHub Projects
- [ ] Projects V2 생성 완료
- [ ] 필드 설정 완료 (Status, Priority, Story Points, Capacity)
- [ ] 테스트 이슈 2-3개 생성
- [ ] 이슈를 프로젝트에 추가
- [ ] 각 이슈의 필드 값 설정 완료

#### Notion
- [ ] Integration 생성 완료
- [ ] API Key 복사 완료
- [ ] 데이터베이스 생성 완료
- [ ] 필수 속성 추가 완료
- [ ] Projects 속성 추가 완료
- [ ] Integration과 데이터베이스 연결 완료
- [ ] Database ID 복사 완료

#### GitHub Repository
- [ ] Secrets 추가 완료:
  - [ ] NOTION_API_KEY
  - [ ] NOTION_DATABASE_ID
- [ ] Workflow permissions 설정 완료
- [ ] 코드 파일 확인:
  - [ ] sync_issues.py
  - [ ] .github/workflows/action.yml
  - [ ] requirements.txt

---

## 첫 번째 동기화 실행

### 방법 1: 수동 실행 (추천)

#### 1. Actions 탭으로 이동

GitHub 레포지토리에서:
```
Code  Issues  Pull requests  Actions  Projects  Settings
                              ^^^^^^^
```

**"Actions"** 탭 클릭

#### 2. Workflow 선택

좌측 사이드바에서:
```
All workflows
  └─ GitHub Notion Issues Sync  ← 클릭
```

#### 3. Run workflow 실행

1. 우측 상단 **"Run workflow"** 버튼 클릭
2. 드롭다운에서:
   ```
   Branch: main
   [Run workflow] 버튼 클릭
   ```

#### 4. 실행 확인

- 새로고침하면 워크플로우 실행이 표시됩니다
- 노란색 원 🟡: 실행 중
- 초록색 체크 ✅: 성공
- 빨간색 X ❌: 실패

#### 5. 로그 확인

워크플로우 실행을 클릭하여 상세 로그 확인:
```
GitHub Notion Issues Sync
  └─ sync
      └─ Sync GitHub Issues to Notion  ← 클릭하면 로그 표시
```

**성공 시 로그 예시:**
```
Run python sync_issues.py
============================================================
GitHub → Notion 이슈 동기화 시작
============================================================
Repository: your-username/issue-sync-test
Notion Database ID: a1b2c3d4...

✓ GitHub에서 3개의 이슈를 가져왔습니다.

동기화 진행 중...
------------------------------------------------------------
  ✓ Issue #1 생성 완료: 로그인 버그 수정
  ✓ Issue #2 생성 완료: 다크모드 UI 구현
  ✓ Issue #3 생성 완료: 성능 최적화

============================================================
동기화 완료!
============================================================
생성됨: 3개
업데이트됨: 0개
실패: 0개
총 처리: 3개
============================================================
```

---

### 방법 2: 이슈 이벤트로 트리거

#### 1. 새 이슈 생성

GitHub에서 새 이슈를 생성하거나 기존 이슈를 수정합니다.

#### 2. Actions 자동 실행

이슈가 생성/수정되면 자동으로 워크플로우가 실행됩니다.

#### 3. Actions 탭에서 확인

Actions 탭을 새로고침하여 실행 중인 워크플로우를 확인합니다.

---

## 결과 확인

### Notion 데이터베이스 확인

1. Notion에서 데이터베이스 페이지 열기
2. 새로고침 (Cmd/Ctrl + R)

**확인할 것들:**

#### 기본 정보가 동기화되었는지
```
예상 결과:

Title: 로그인 버그 수정
Issue Number: 1
Status: Open
Labels: bug, urgent
URL: https://github.com/your-username/your-repo/issues/1
Created At: 2024-01-15
Assignee: your-username

[페이지 본문]
# 문제 설명
로그인 시 타임아웃 발생
...
```

#### Markdown이 제대로 변환되었는지

페이지를 열어서 확인:
- ✅ 헤딩이 Heading 블록으로 변환
- ✅ 코드 블록이 Code 블록으로 변환
- ✅ 리스트가 Bulleted List로 변환
- ✅ 체크박스가 To-do 블록으로 변환

#### Projects 정보가 동기화되었는지 (현재는 아직 구현 전)

⚠️ **주의**: Projects 연동은 아직 구현하지 않았습니다!

현재 동기화되는 것:
- ✅ 이슈 제목, 번호, 상태, 라벨, URL
- ✅ 이슈 본문 (Markdown → Notion 블록)
- ✅ 생성일, 담당자

아직 동기화되지 않는 것:
- ❌ Project 이름
- ❌ Project Status
- ❌ Priority, Story Points, Capacity 등

→ 다음 단계에서 구현 예정!

---

## 자동 동기화 확인

### 이슈 생성 시

1. GitHub에서 새 이슈 생성
2. 잠시 대기 (1-2분)
3. Actions 탭에서 워크플로우 실행 확인
4. Notion에서 새 이슈 확인

### 이슈 수정 시

1. GitHub 이슈 제목 또는 본문 수정
2. 잠시 대기
3. Actions 탭에서 워크플로우 실행 확인
4. Notion에서 업데이트된 내용 확인

### 이슈 닫기 시

1. GitHub 이슈 Close
2. Actions 탭에서 워크플로우 실행 확인
3. Notion에서 Status가 "Closed"로 변경되었는지 확인

### 스케줄 동기화

매 시간마다 자동 실행:
- 다음 정각에 Actions 탭 확인
- 스케줄에 의해 자동 실행되는지 확인

---

## 성능 확인

### 동기화 속도

이슈 수에 따른 예상 소요 시간:

| 이슈 수 | 소요 시간 |
|---------|-----------|
| 1-10개 | 10-30초 |
| 10-50개 | 30초-2분 |
| 50-100개 | 2-5분 |

### Notion API 제한

Notion API는 다음 제한이 있습니다:
- 초당 3 requests
- 많은 이슈가 있으면 시간이 걸릴 수 있음

---

## 문제 해결

### 에러: "GITHUB_REPOSITORY 환경 변수가 설정되지 않았습니다"

**원인:** GitHub Actions 환경에서 실행되지 않음

**해결:**
- Actions 탭에서 실행했는지 확인
- 로컬에서 실행하려면 환경 변수 설정 필요

### 에러: "NOTION_API_KEY 환경 변수가 설정되지 않았습니다"

**원인:** Secret이 설정되지 않음

**해결:**
1. Settings → Secrets and variables → Actions
2. `NOTION_API_KEY` Secret 확인
3. 없으면 추가

### 에러: "✗ Notion API 호출 실패: 401 Unauthorized"

**원인:** Notion API Key가 올바르지 않음

**해결:**
1. Notion Integration Token 재확인
2. Secret 업데이트
3. Integration이 데이터베이스에 연결되었는지 확인

### 에러: "✗ Notion API 호출 실패: 404 Not Found"

**원인:** Database ID가 올바르지 않음

**해결:**
1. Database ID 재확인 (32자리)
2. 하이픈(-) 제거했는지 확인
3. Secret 업데이트

### 에러: "✗ GitHub API 호출 실패: 403 Forbidden"

**원인:** GitHub Token 권한 부족

**해결:**
1. Settings → Actions → General
2. Workflow permissions를 "Read and write"로 변경
3. Save

### 이슈가 동기화되지 않음

**확인 사항:**
1. Actions 탭에서 워크플로우가 실행되었는지 확인
2. 로그에서 에러 메시지 확인
3. Notion에서 새로고침 (Cmd/Ctrl + R)
4. Issue Number로 검색해보기

### Markdown이 제대로 변환되지 않음

**알려진 이슈:**
- 복잡한 중첩 구조는 간단하게 변환될 수 있음
- 이미지는 URL 텍스트로 표시됨
- 표(table)는 일반 텍스트로 표시됨

**개선 예정!**

### 워크플로우가 자동 실행되지 않음

**확인:**
1. `.github/workflows/action.yml` 파일 확인
2. `on:` 섹션에 `issues` 트리거가 있는지 확인
3. Repository Settings → Actions → General
4. Actions permissions가 활성화되어 있는지 확인

---

## 다음 단계

✅ 기본 동기화 테스트 완료!

### 추가 기능 구현

현재는 기본 이슈 정보만 동기화됩니다. 다음 기능들을 추가할 예정:

#### 📋 To-do:
- [ ] **여러 레포 지원** - Organization의 여러 레포 동기화
- [ ] **Projects V2 연동** - Projects 정보 가져오기
  - [ ] Project 이름
  - [ ] Project Status (Backlog, Todo, In Progress, Done)
  - [ ] Priority
  - [ ] Story Points, Capacity
  - [ ] Sprint
- [ ] **설정 파일** - config.yml로 레포 목록 관리
- [ ] **GraphQL 구현** - Projects API 호출

---

## 로그 분석

### 성공 로그 예시

```
============================================================
GitHub → Notion 이슈 동기화 시작
============================================================
Repository: jangjunho/issue-sync-test
Notion Database ID: a1b2c3d4...

✓ GitHub에서 3개의 이슈를 가져왔습니다.

동기화 진행 중...
------------------------------------------------------------
  ✓ Issue #1 생성 완료: 로그인 버그 수정
  ✓ Issue #2 생성 완료: 다크모드 UI 구현
  ✓ Issue #3 업데이트 완료: 성능 최적화

============================================================
동기화 완료!
============================================================
생성됨: 2개
업데이트됨: 1개
실패: 0개
총 처리: 3개
============================================================
```

### 실패 로그 예시

```
✗ GitHub API 호출 실패: 403 Forbidden
```

→ 권한 문제, Workflow permissions 확인

```
✗ Notion API 호출 실패: 401 Unauthorized
```

→ NOTION_API_KEY 확인

```
  ✗ Issue #5 생성 실패: 400 Bad Request
    에러 상세: {"message": "body failed validation"}
```

→ Notion 데이터베이스 속성 확인

---

## 테스트 시나리오

### 시나리오 1: 새 이슈 생성
1. GitHub에서 이슈 생성
2. Markdown 본문 작성 (헤딩, 코드, 리스트 등)
3. 라벨 추가
4. Projects에 추가
5. Actions에서 동기화 확인
6. Notion에서 모든 정보 확인

### 시나리오 2: 이슈 수정
1. GitHub 이슈 제목 변경
2. 본문 수정
3. 라벨 변경
4. Actions에서 동기화 확인
5. Notion에서 업데이트 확인

### 시나리오 3: 이슈 닫기
1. GitHub 이슈 Close
2. Actions에서 동기화 확인
3. Notion Status가 "Closed"로 변경 확인

### 시나리오 4: 대량 이슈
1. GitHub에서 이슈 10개 생성
2. 수동으로 workflow 실행
3. 모든 이슈가 동기화되는지 확인
4. 소요 시간 확인

---

## 성공 기준

다음이 모두 확인되면 테스트 성공입니다:

- ✅ GitHub 이슈가 Notion에 생성됨
- ✅ 제목, 번호, 상태, 라벨이 정확히 동기화됨
- ✅ Markdown 본문이 Notion 블록으로 변환됨
- ✅ 이슈 수정 시 Notion도 업데이트됨
- ✅ 이슈 닫기 시 Status가 Closed로 변경됨
- ✅ 자동 동기화가 작동함 (이벤트, 스케줄)
- ✅ 에러 없이 안정적으로 실행됨

축하합니다! 🎉 기본 동기화가 성공적으로 작동합니다!

다음으로 Projects V2 연동을 구현하겠습니다.

