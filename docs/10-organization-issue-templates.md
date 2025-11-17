# 🏢 Organization 공용 이슈 템플릿 설정

이 문서는 GitHub Organization 전체에서 사용할 수 있는 공용 이슈 템플릿을 만드는 방법을 설명합니다.

## 목차
- [개요](#개요)
- [.github 레포지토리 생성](#github-레포지토리-생성)
- [이슈 템플릿 생성](#이슈-템플릿-생성)
- [템플릿 예시](#템플릿-예시)
- [적용 확인](#적용-확인)
- [주의사항 및 제한사항](#주의사항-및-제한사항)
- [동기화 스크립트와 연계](#동기화-스크립트와-연계)

---

## 개요

### Organization 공용 템플릿이란?

GitHub Organization에 **`.github`** 이름의 특별한 레포지토리를 만들면, Organization 내 모든 레포지토리에서 공용 커뮤니티 파일을 공유할 수 있습니다.

### 공유 가능한 파일들

```
.github/
├── ISSUE_TEMPLATE/          # 이슈 템플릿 ⭐
│   ├── bug_report.yml
│   ├── feature_request.yml
│   └── config.yml
├── PULL_REQUEST_TEMPLATE.md  # PR 템플릿
├── CODE_OF_CONDUCT.md        # 행동 강령
├── CONTRIBUTING.md           # 기여 가이드
├── SECURITY.md               # 보안 정책
├── SUPPORT.md                # 지원 정보
└── FUNDING.yml               # 스폰서십 정보
```

### 동작 원리

```
Organization: MyOrg
├── .github (템플릿 저장소)
│   └── ISSUE_TEMPLATE/bug_report.yml
│
├── project-A (템플릿 없음)
│   → .github의 템플릿 자동 사용 ✅
│
├── project-B (자체 템플릿 있음)
│   └── .github/ISSUE_TEMPLATE/custom.yml
│   → 자체 템플릿 사용 (우선순위 높음) ⚠️
│
└── project-C (템플릿 없음)
    → .github의 템플릿 자동 사용 ✅
```

---

## .github 레포지토리 생성

### 1. Organization 페이지로 이동

```
https://github.com/YOUR_ORG
```

### 2. 새 레포지토리 생성

- **"New repository"** 버튼 클릭
- **Repository name**: `.github` (정확히 이 이름으로!)
- **Description**: `Organization-wide community health files`
- **Public** 또는 **Private** 선택
  - ⚠️ Private Organization은 기능이 제한적일 수 있음
- **Initialize this repository with:**
  - ✅ Add a README file (선택사항)
- **"Create repository"** 클릭

### 3. 레포지토리 클론 (선택사항)

로컬에서 작업하려면:

```bash
cd ~/dev
git clone https://github.com/YOUR_ORG/.github.git
cd .github
```

---

## 이슈 템플릿 생성

### 1. 디렉터리 구조 생성

**방법 A: GitHub 웹에서**

1. `.github` 레포지토리로 이동
2. **"Add file" → "Create new file"** 클릭
3. 파일명 입력: `.github/ISSUE_TEMPLATE/bug_report.yml`
   - `/`를 입력하면 자동으로 디렉터리가 생성됨

**방법 B: 로컬에서**

```bash
# .github 레포지토리 안에서
mkdir -p .github/ISSUE_TEMPLATE
cd .github/ISSUE_TEMPLATE
```

### 2. 템플릿 파일 작성

GitHub는 두 가지 템플릿 형식을 지원합니다:

| 형식 | 파일 확장자 | 특징 |
|------|-----------|------|
| **Form (권장)** | `.yml` | 구조화된 입력 폼, 검증 가능 |
| **Markdown** | `.md` | 자유로운 형식, 간단 |

**Form 템플릿 (`.yml`)을 권장합니다!**

### 3. config.yml 생성 (선택사항)

템플릿 설정 파일로, 템플릿 선택 화면을 커스터마이즈할 수 있습니다.

```yaml
# .github/ISSUE_TEMPLATE/config.yml
blank_issues_enabled: false  # 빈 이슈 생성 비활성화
contact_links:
  - name: 💬 Community Discussion
    url: https://github.com/orgs/YOUR_ORG/discussions
    about: 질문은 여기에 남겨주세요
  - name: 📚 Documentation
    url: https://docs.your-org.com
    about: 공식 문서를 확인하세요
```

---

## 템플릿 예시

### 📝 버그 리포트 템플릿

파일: `.github/ISSUE_TEMPLATE/bug_report.yml`

```yaml
name: 🐛 버그 리포트
description: 버그를 발견하셨나요? 자세히 알려주세요!
title: "[BUG] "
labels: ["bug", "triage"]
assignees:
  - maintainer-username

body:
  - type: markdown
    attributes:
      value: |
        ## 버그를 리포트해주셔서 감사합니다!
        
        가능한 한 자세히 작성해주시면 빠른 해결에 도움이 됩니다.

  - type: textarea
    id: description
    attributes:
      label: 🐛 버그 설명
      description: 어떤 문제가 발생했나요?
      placeholder: 로그인 버튼을 클릭해도 아무 반응이 없습니다.
    validations:
      required: true

  - type: textarea
    id: reproduction
    attributes:
      label: 🔄 재현 방법
      description: 버그를 재현할 수 있는 단계를 알려주세요
      placeholder: |
        1. 웹사이트 접속
        2. 로그인 페이지 이동
        3. 아이디/비밀번호 입력
        4. 로그인 버튼 클릭
        5. 아무 반응 없음
    validations:
      required: true

  - type: textarea
    id: expected
    attributes:
      label: ✅ 예상 동작
      description: 정상적으로 작동한다면 어떻게 되어야 하나요?
      placeholder: 로그인 후 대시보드로 이동해야 합니다.
    validations:
      required: true

  - type: textarea
    id: actual
    attributes:
      label: ❌ 실제 동작
      description: 실제로는 어떻게 동작하나요?
      placeholder: 버튼을 클릭해도 아무 일도 일어나지 않습니다.
    validations:
      required: true

  - type: dropdown
    id: severity
    attributes:
      label: 🚨 심각도
      description: 이 버그가 얼마나 심각한가요?
      options:
        - 🔴 Critical - 서비스 사용 불가
        - 🟠 High - 주요 기능 동작 안 함
        - 🟡 Medium - 일부 기능에 문제
        - 🟢 Low - 사소한 문제
    validations:
      required: true

  - type: dropdown
    id: browser
    attributes:
      label: 🌐 브라우저 (해당되는 경우)
      options:
        - Chrome
        - Firefox
        - Safari
        - Edge
        - 기타
        - 해당 없음
    validations:
      required: false

  - type: input
    id: version
    attributes:
      label: 📦 버전
      description: 사용 중인 버전이 무엇인가요? (예: v1.2.3)
      placeholder: v1.2.3
    validations:
      required: false

  - type: textarea
    id: logs
    attributes:
      label: 📋 로그 및 에러 메시지
      description: 콘솔 로그나 에러 메시지를 붙여넣어주세요
      placeholder: |
        Uncaught TypeError: Cannot read property 'user' of undefined
      render: shell
    validations:
      required: false

  - type: textarea
    id: screenshots
    attributes:
      label: 📸 스크린샷
      description: 스크린샷이 있다면 여기에 드래그해주세요
    validations:
      required: false

  - type: textarea
    id: additional
    attributes:
      label: 📝 추가 정보
      description: 기타 도움이 될 만한 정보가 있나요?
    validations:
      required: false

  - type: checkboxes
    id: terms
    attributes:
      label: ✅ 체크리스트
      description: 제출하기 전에 확인해주세요
      options:
        - label: 기존 이슈를 검색했고, 중복이 아닙니다
          required: true
        - label: 최신 버전에서 문제가 발생합니다
          required: false
```

### ✨ 기능 제안 템플릿

파일: `.github/ISSUE_TEMPLATE/feature_request.yml`

```yaml
name: ✨ 기능 제안
description: 새로운 기능을 제안해주세요!
title: "[FEATURE] "
labels: ["enhancement", "triage"]

body:
  - type: markdown
    attributes:
      value: |
        ## 새로운 아이디어를 환영합니다! 🎉
        
        구체적으로 설명해주시면 검토에 도움이 됩니다.

  - type: textarea
    id: problem
    attributes:
      label: 🤔 어떤 문제를 해결하나요?
      description: 현재 불편한 점이나 부족한 점을 설명해주세요
      placeholder: 대량의 데이터를 내보낼 때 시간이 너무 오래 걸립니다.
    validations:
      required: true

  - type: textarea
    id: solution
    attributes:
      label: 💡 제안하는 해결 방법
      description: 어떤 기능이 추가되면 좋을까요?
      placeholder: 백그라운드 작업 큐를 추가하여 비동기로 처리하면 좋겠습니다.
    validations:
      required: true

  - type: textarea
    id: alternatives
    attributes:
      label: 🔄 대안
      description: 다른 방법은 없을까요?
      placeholder: 데이터를 압축해서 다운로드 크기를 줄이는 방법도 있습니다.
    validations:
      required: false

  - type: dropdown
    id: priority
    attributes:
      label: ⚡ 우선순위
      description: 이 기능이 얼마나 중요한가요?
      options:
        - 🔴 높음 - 핵심 기능
        - 🟡 중간 - 있으면 좋음
        - 🟢 낮음 - Nice to have
    validations:
      required: true

  - type: checkboxes
    id: willingness
    attributes:
      label: 🙋 기여 의향
      options:
        - label: 이 기능을 직접 구현할 의향이 있습니다 (PR)
          required: false

  - type: textarea
    id: additional
    attributes:
      label: 📝 추가 정보
      description: 참고 자료, 스크린샷, 다른 프로젝트 예시 등
    validations:
      required: false
```

### ❓ 질문 템플릿

파일: `.github/ISSUE_TEMPLATE/question.yml`

```yaml
name: ❓ 질문
description: 사용 방법이나 기술적인 질문이 있으신가요?
title: "[QUESTION] "
labels: ["question"]

body:
  - type: markdown
    attributes:
      value: |
        ## 질문하기 전에 확인해주세요 📚
        
        - [문서](https://docs.example.com)를 먼저 확인해보셨나요?
        - [FAQ](https://github.com/YOUR_ORG/.github/wiki/FAQ)를 읽어보셨나요?

  - type: textarea
    id: question
    attributes:
      label: ❓ 질문 내용
      description: 무엇이 궁금하신가요?
      placeholder: 특정 기능을 어떻게 사용하나요?
    validations:
      required: true

  - type: dropdown
    id: category
    attributes:
      label: 📂 카테고리
      options:
        - 설치 및 설정
        - 사용 방법
        - 에러 해결
        - 성능 최적화
        - 기타
    validations:
      required: true

  - type: textarea
    id: tried
    attributes:
      label: 🔍 시도해본 것
      description: 이미 시도해본 방법이 있나요?
      placeholder: 문서를 읽어봤지만 특정 부분이 이해가 안 갑니다.
    validations:
      required: false

  - type: textarea
    id: context
    attributes:
      label: 📋 사용 환경
      description: 환경 정보를 알려주세요 (버전, OS 등)
      placeholder: |
        - 버전: v1.2.3
        - OS: macOS 14.0
        - 언어: Python 3.11
    validations:
      required: false
```

### 📖 Markdown 템플릿 (간단한 버전)

Form이 아닌 Markdown 템플릿을 원하는 경우:

파일: `.github/ISSUE_TEMPLATE/simple_bug.md`

```markdown
---
name: 간단한 버그 리포트
about: 간단하게 버그를 보고합니다
title: '[BUG] '
labels: 'bug'
assignees: ''
---

## 버그 설명
<!-- 어떤 문제가 발생했나요? -->

## 재현 방법
1. 
2. 
3. 

## 예상 동작
<!-- 정상적으로는 어떻게 동작해야 하나요? -->

## 실제 동작
<!-- 실제로는 어떻게 동작하나요? -->

## 스크린샷
<!-- 스크린샷이 있다면 첨부해주세요 -->

## 환경
- OS: [예: macOS, Windows, Linux]
- 브라우저: [예: Chrome, Firefox]
- 버전: [예: v1.2.3]
```

---

## 적용 확인

### 1. 템플릿 커밋 및 푸시

```bash
git add .github/ISSUE_TEMPLATE/
git commit -m "Add organization-wide issue templates"
git push origin main
```

### 2. Organization의 다른 레포에서 테스트

1. **Organization 내 다른 레포로 이동** (자체 템플릿이 **없는** 레포)
2. **"Issues" 탭** 클릭
3. **"New issue"** 버튼 클릭
4. ✅ `.github` 레포의 템플릿이 보이면 성공!

### 3. 템플릿 선택 화면 예시

정상적으로 적용되면 다음과 같이 보입니다:

```
Get started
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🐛 버그 리포트
버그를 발견하셨나요? 자세히 알려주세요!
[Get started]

✨ 기능 제안
새로운 기능을 제안해주세요!
[Get started]

❓ 질문
사용 방법이나 기술적인 질문이 있으신가요?
[Get started]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Don't see your issue here? Open a blank issue.
```

### 4. 레포별 템플릿 확인 방법

어떤 템플릿을 사용하는지 확인:

```bash
# Organization의 모든 레포 순회
for repo in project-A project-B project-C; do
  echo "=== $repo ==="
  gh api repos/YOUR_ORG/$repo/contents/.github/ISSUE_TEMPLATE 2>/dev/null \
    && echo "✅ 자체 템플릿 있음" \
    || echo "📋 Organization 템플릿 사용"
done
```

---

## 주의사항 및 제한사항

### ⚠️ 우선순위 규칙

| 순위 | 템플릿 위치 | 설명 |
|------|----------|------|
| 1 | `repository/.github/ISSUE_TEMPLATE/` | 개별 레포의 템플릿 (최우선) |
| 2 | `organization/.github/ISSUE_TEMPLATE/` | Organization 공용 템플릿 |
| 3 | 없음 | 빈 이슈만 생성 가능 |

**개별 레포에 템플릿이 있으면 Organization 템플릿은 무시됩니다!**

### 🔒 Private Organization 제한사항

- **Private Organization**의 경우 일부 기능이 제한적일 수 있음
- Organization 멤버만 템플릿을 볼 수 있음
- 외부 기여자는 템플릿을 볼 수 없을 수 있음

### 📝 템플릿 적용 타이밍

- **즉시 반영**: `.github` 레포의 변경사항은 즉시 적용됨
- 캐시가 있을 수 있으니 안 보이면 **새로고침** 또는 **시크릿 모드**에서 확인

### 🚫 적용되지 않는 경우

다음 경우에는 Organization 템플릿이 적용되지 않습니다:

- ❌ 개별 레포에 이미 자체 템플릿이 있는 경우
- ❌ `.github` 레포가 Private이고 접근 권한이 없는 경우
- ❌ `.github` 레포 이름이 정확하지 않은 경우 (대소문자 구분!)

### 📏 템플릿 제한사항

GitHub Form 템플릿의 제한사항:

| 항목 | 제한 |
|------|------|
| 최대 필드 수 | 35개 |
| 최대 옵션 수 (dropdown) | 25개 |
| 템플릿 파일 크기 | 256KB |
| 라벨 수 | 제한 없음 (하지만 적절히) |

---

## 동기화 스크립트와 연계

Organization 공용 템플릿을 사용하면, 이슈 동기화 스크립트에서 템플릿 정보를 활용할 수 있습니다.

### 1. Notion 데이터베이스에 필드 추가

| 필드 이름 | 타입 | 옵션 |
|---------|------|------|
| **Issue Template** | Select | bug-report, feature-request, question |
| **Template Sections** | Text | JSON 형식 섹션 데이터 |

### 2. 라벨 기반 템플릿 식별

템플릿에서 자동으로 추가되는 라벨을 이용:

```python
def get_template_type(issue: Dict) -> Optional[str]:
    """라벨을 통해 템플릿 타입을 식별합니다"""
    labels = [label["name"] for label in issue.get("labels", [])]
    
    # 템플릿별 라벨 매핑
    template_map = {
        "bug": "bug-report",
        "enhancement": "feature-request",
        "question": "question"
    }
    
    for label in labels:
        if label in template_map:
            return template_map[label]
    
    return None
```

### 3. 본문 파싱으로 템플릿 정보 추출

Form 템플릿의 섹션은 이슈 본문에 구조화되어 저장됩니다:

```python
def parse_template_sections(issue: Dict) -> Dict[str, str]:
    """이슈 본문에서 템플릿 섹션을 파싱합니다"""
    body = issue.get("body", "")
    sections = {}
    
    # Form 템플릿 형식: ### 섹션명\n내용
    pattern = r'###\s+(.+?)\n\n(.*?)(?=\n###|\Z)'
    matches = re.findall(pattern, body, re.DOTALL)
    
    for section_name, content in matches:
        sections[section_name.strip()] = content.strip()
    
    return sections

# 사용 예시
sections = parse_template_sections(issue)
# {
#   "🐛 버그 설명": "로그인 버튼이 동작하지 않습니다",
#   "🔄 재현 방법": "1. 로그인 페이지 접속\n2. 버튼 클릭",
#   "✅ 예상 동작": "로그인 후 대시보드로 이동"
# }
```

### 4. sync_issues.py에 통합

`sync_issues.py`의 `create_notion_page` 함수에 추가:

```python
# 템플릿 정보 추가
template_type = get_template_type(issue)
if template_type:
    data["properties"]["Issue Template"] = {
        "select": {
            "name": template_type
        }
    }

# 템플릿 섹션 추가 (JSON 형식)
template_sections = parse_template_sections(issue)
if template_sections:
    data["properties"]["Template Sections"] = {
        "rich_text": [
            {
                "text": {
                    "content": json.dumps(template_sections, ensure_ascii=False)
                }
            }
        ]
    }
```

---

## 추가 리소스

### 📚 공식 문서

- [GitHub Issue Forms Syntax](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms)
- [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
- [Configuring issue templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository)

### 🎯 실제 예시

GitHub Organization의 `.github` 레포 예시:

- [github/.github](https://github.com/github/.github) - GitHub 공식
- [microsoft/.github](https://github.com/microsoft/.github) - Microsoft
- [facebook/.github](https://github.com/facebook/.github) - Meta

### 🛠 도구

- [Issue Forms Creator](https://issue-forms-creator.netlify.app/) - 웹 기반 폼 생성기
- [GitHub CLI](https://cli.github.com/) - `gh` 명령어로 템플릿 관리

---

## 다음 단계

✅ Organization 공용 템플릿 설정 완료!

이제 다음을 진행할 수 있습니다:

1. **[동기화 스크립트 업데이트](../README.md#사용-방법)** - 템플릿 정보 동기화 추가
2. **[Notion 데이터베이스 필드 추가](./02-notion-setup.md)** - Issue Template 필드 생성
3. **[팀원에게 공유](./03-github-secrets.md)** - 템플릿 사용 가이드 배포

---

**⏱️ 예상 소요 시간**: 30-45분
- `.github` 레포 생성: 5분
- 템플릿 작성: 20-30분
- 테스트 및 확인: 5-10분

**💡 팁**: 템플릿은 처음에는 간단하게 만들고, 팀의 피드백을 받아 점진적으로 개선하는 것이 좋습니다!

