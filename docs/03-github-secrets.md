# 🔐 Step 3: GitHub Secrets 설정

이 문서는 GitHub Repository Secrets를 설정하는 방법을 설명합니다.

## 목차
- [Secrets란?](#secrets란)
- [필요한 Secrets](#필요한-secrets)
- [Secrets 추가 방법](#secrets-추가-방법)
- [권한 설정](#권한-설정)
- [확인 사항](#확인-사항)

---

## Secrets란?

GitHub Secrets는 API Key, Token 등 민감한 정보를 안전하게 저장하는 곳입니다.

**특징:**
- ✅ 암호화되어 저장
- ✅ GitHub Actions에서 사용 가능
- ✅ 코드에 노출되지 않음
- ⚠️ 한번 저장하면 값을 볼 수 없음 (수정만 가능)

---

## 필요한 Secrets

동기화를 위해 다음 2개의 Secrets가 필요합니다:

| Secret 이름 | 설명 | 값 |
|-------------|------|-----|
| `NOTION_API_KEY` | Notion Integration API Key | `secret_...` |
| `NOTION_DATABASE_ID` | Notion Database ID | 32자리 영숫자 |

⚠️ **GITHUB_TOKEN**은 자동으로 제공되므로 추가할 필요 없습니다!

---

## Secrets 추가 방법

### 1. Repository Settings로 이동

1. GitHub 레포지토리 페이지 접속
2. 상단 메뉴에서 **"Settings"** 클릭

```
Code  Issues  Pull requests  Actions  Projects  Settings
                                                  ^^^^^^^^
```

### 2. Secrets and variables 메뉴

좌측 사이드바에서:
1. **"Secrets and variables"** 클릭
2. **"Actions"** 클릭

```
좌측 메뉴:
  Security
    ↓
  Secrets and variables
    → Actions  ← 여기
```

### 3. New repository secret 클릭

**"New repository secret"** 버튼 클릭 (초록색 버튼)

---

## Secret 1: NOTION_API_KEY

### 추가하기

1. **"New repository secret"** 클릭
2. 정보 입력:

```
Name: NOTION_API_KEY

Secret: secret_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
        ↑ Step 2에서 복사한 Notion Integration Token
```

3. **"Add secret"** 버튼 클릭

### ✅ 확인

Secrets 목록에 `NOTION_API_KEY`가 추가되었는지 확인:

```
Repository secrets

NOTION_API_KEY        Updated now
```

---

## Secret 2: NOTION_DATABASE_ID

### 추가하기

1. **"New repository secret"** 클릭 (다시)
2. 정보 입력:

```
Name: NOTION_DATABASE_ID

Secret: a1b2c3d4e5f67890a1b2c3d4e5f67890
        ↑ Step 2에서 복사한 Database ID (하이픈 제거)
```

3. **"Add secret"** 버튼 클릭

### ✅ 확인

Secrets 목록에 2개가 있는지 확인:

```
Repository secrets

NOTION_API_KEY         Updated now
NOTION_DATABASE_ID     Updated now
```

---

## 권한 설정

GitHub Actions가 이슈와 프로젝트에 접근할 수 있도록 권한을 확인합니다.

### 1. Actions Permissions로 이동

Settings 좌측 사이드바:
1. **"Actions"** 클릭
2. **"General"** 클릭

### 2. Workflow permissions 확인

페이지 하단의 **"Workflow permissions"** 섹션으로 스크롤

다음 중 하나를 선택:

#### 옵션 1: Read and write permissions (추천)
```
⚪ Read repository contents and packages permissions
🔘 Read and write permissions
```

이 옵션을 선택하면 추가 설정이 필요 없습니다.

#### 옵션 2: Read repository contents (권장)
```
🔘 Read repository contents and packages permissions
⚪ Read and write permissions
```

이 옵션을 선택하면 workflow 파일에서 개별 권한을 지정해야 합니다 (이미 설정되어 있음).

### 3. Save 클릭

**"Save"** 버튼 클릭

---

## Projects 권한 추가 (Projects V2 사용 시)

현재 workflow 파일에 이미 설정되어 있지만, 확인해봅시다.

### workflow 파일 확인

`.github/workflows/action.yml` 파일을 확인:

```yaml
permissions:
  issues: read
  contents: read
  # Projects V2 사용 시 필요:
  # repository-projects: read
```

### Projects 권한 활성화 (나중에)

Projects V2 연동을 구현할 때 다음을 추가할 예정:

```yaml
permissions:
  issues: read
  contents: read
  repository-projects: read  # ← 이것 추가
```

⚠️ **지금은 추가하지 않아도 됩니다!** 나중에 코드와 함께 추가하겠습니다.

---

## Personal Access Token (PAT) - 고급

**개인 레포에서는 필요 없습니다!**

만약 나중에 Organization의 다른 레포들을 동기화하려면 PAT이 필요할 수 있습니다.

### PAT이 필요한 경우:
- ❌ 개인 레포 → 필요 없음 (GITHUB_TOKEN 충분)
- ✅ Organization 레포 → 필요할 수 있음
- ✅ Private 레포 여러 개 동기화 → 필요할 수 있음

### PAT 생성 (나중에 필요 시)

1. GitHub Settings (개인 설정) → Developer settings
2. Personal access tokens → Fine-grained tokens
3. Generate new token
4. 권한 선택:
   - Repository access: 접근할 레포 선택
   - Permissions:
     - Issues: Read
     - Contents: Read
     - Projects: Read (V2 사용 시)
5. Generate token
6. Token을 Secret으로 추가: `GITHUB_PAT`

⚠️ **지금은 하지 마세요!** 개인 레포 테스트에는 필요 없습니다.

---

## 확인 사항

### ✅ 체크리스트

- [ ] Repository Settings 접속 완료
- [ ] Secrets and variables → Actions 페이지 접속
- [ ] `NOTION_API_KEY` Secret 추가 완료
- [ ] `NOTION_DATABASE_ID` Secret 추가 완료
- [ ] Secrets 목록에 2개 표시 확인
- [ ] Workflow permissions 확인 (Read and write 또는 Read repository)
- [ ] Save 클릭

### 📋 최종 Secret 목록

```
Repository secrets (2)

NOTION_API_KEY         Updated now
NOTION_DATABASE_ID     Updated now
```

### 🔐 보안 체크

- [ ] Notion API Key를 코드에 넣지 않았는지 확인
- [ ] Database ID를 코드에 넣지 않았는지 확인
- [ ] `.env` 파일이 있다면 `.gitignore`에 추가했는지 확인
- [ ] 민감한 정보가 커밋되지 않았는지 확인

---

## 다음 단계

✅ GitHub Secrets 설정 완료!

➡️ **다음**: [Step 4: 테스트 실행](./04-testing.md)

---

## 문제 해결

### Q: Settings 메뉴가 안 보여요
**A:** 
- 레포지토리 소유자 또는 Admin 권한이 필요합니다
- 본인의 개인 레포인지 확인하세요

### Q: Secret을 잘못 입력했어요
**A:**
1. Secret 이름 클릭
2. "Update secret" 버튼으로 수정
3. 새 값 입력 후 Update

### Q: GITHUB_TOKEN은 어디에 추가하나요?
**A:**
- GITHUB_TOKEN은 자동으로 제공됩니다
- 별도로 추가할 필요 없습니다
- workflow에서 `${{ secrets.GITHUB_TOKEN }}`로 사용 가능

### Q: Secret 값을 확인하고 싶어요
**A:**
- Secret은 추가 후 값을 볼 수 없습니다 (보안상)
- 값이 맞는지 확인하려면:
  1. 워크플로우 실행해보기
  2. 실패하면 Secret 재생성

### Q: Actions가 "Resource not accessible by integration" 에러
**A:**
1. Settings → Actions → General
2. Workflow permissions를 "Read and write"로 변경
3. Save
4. 워크플로우 재실행

### Q: 동기화 시 "401 Unauthorized" 에러 (Notion)
**A:**
- NOTION_API_KEY가 올바른지 확인
- Integration Token을 재생성했다면 Secret도 업데이트
- Integration이 데이터베이스에 연결되었는지 확인

### Q: 동기화 시 "404 Not Found" 에러 (Notion)
**A:**
- NOTION_DATABASE_ID가 올바른지 확인
- Database ID에서 하이픈(-) 제거했는지 확인
- Database ID가 32자리인지 확인

