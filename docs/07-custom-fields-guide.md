# 🎨 커스텀 필드 추가 가이드

GitHub Projects나 Notion에 커스텀 필드를 추가했을 때 동기화 코드를 수정하는 방법입니다.

## 목차
- [개요](#개요)
- [Notion에 새 속성 추가](#notion에-새-속성-추가)
- [GitHub Projects에 새 필드 추가](#github-projects에-새-필드-추가)
- [코드 수정 방법](#코드-수정-방법)
- [필드 타입별 예시](#필드-타입별-예시)
- [테스트](#테스트)

---

## 개요

### 현재 동기화되는 필드

#### GitHub Issue 기본 정보:
- Title, Issue Number, Status, Labels
- URL, Created At, Assignee, Repository

#### GitHub Projects V2 필드:
- Project (프로젝트 이름)
- Status (Backlog, Ready, In progress, In review, Done)
- Priority (Critical, High, Medium, Low)
- Story Points, Capacity, Sprint

### 커스텀 필드 추가 시나리오

**예시 1:** GitHub Projects에 `Due Date` 필드 추가
```
GitHub Projects: Due Date (Date 타입)
→ Notion에도 동기화하고 싶음
```

**예시 2:** Notion에 `Reviewer` 속성 추가
```
Notion: Reviewer (Text 타입)
→ GitHub의 assignee 정보로 채우고 싶음
```

**예시 3:** GitHub Projects에 `Team` 필드 추가
```
GitHub Projects: Team (Single Select: Frontend, Backend, Mobile)
→ Notion에 동기화
```

---

## Notion에 새 속성 추가

### 1. Notion 데이터베이스 열기

동기화 중인 Notion 데이터베이스로 이동

### 2. 새 속성 추가

데이터베이스 우측 **"+"** 버튼 또는 열 헤더의 **"+"** 클릭

### 3. 속성 설정

#### 예시: Team 필드 추가

```
Property name: Team
Property type: Select

Options 추가:
  - Frontend
  - Backend
  - Mobile
  - DevOps
```

**완료!** Notion 준비 끝.

---

## GitHub Projects에 새 필드 추가

### 1. Projects 열기

GitHub에서 Projects V2 화면으로 이동

### 2. 새 필드 추가

우측 상단 **"+"** 버튼 클릭 → **"New field"**

### 3. 필드 설정

#### 예시: Team 필드 추가

```
Field name: Team
Field type: Single select

Options:
  - Frontend
  - Backend
  - Mobile
  - DevOps
```

**완료!** GitHub Projects 준비 끝.

---

## 코드 수정 방법

### 📍 수정할 파일: sync_issues.py

---

## 필드 타입별 예시

### 타입 1: Single Select (Select)

**GitHub Projects:** Team (Frontend/Backend/Mobile)  
**Notion:** Team (Select)

#### 코드 위치: `create_notion_page()` 함수

```python
# sync_issues.py 약 595~650번 줄 부근

# Projects V2 정보 조회 및 추가
projects_info = self.get_issue_projects_info(issue)
if projects_info:
    fields = projects_info.get("fields", {})
    
    # 기존 필드들 (이미 있음)
    if "Status" in fields:
        data["properties"]["Project Status"] = { ... }
    
    if "Priority" in fields:
        data["properties"]["Priority"] = { ... }
    
    # ✨ 새로 추가: Team 필드
    if "Team" in fields:
        data["properties"]["Team"] = {
            "select": {
                "name": fields["Team"]
            }
        }
```

#### update_notion_page()에도 동일하게 추가

```python
# sync_issues.py 약 730~780번 줄 부근

# Projects V2 정보 업데이트
projects_info = self.get_issue_projects_info(issue)
if projects_info:
    fields = projects_info.get("fields", {})
    
    # ✨ 새로 추가: Team 필드
    if "Team" in fields:
        data["properties"]["Team"] = {
            "select": {
                "name": fields["Team"]
            }
        }
```

---

### 타입 2: Number (숫자)

**GitHub Projects:** Estimated Hours (숫자)  
**Notion:** Estimated Hours (Number)

#### 코드 추가:

```python
# create_notion_page()와 update_notion_page() 둘 다

if "Estimated Hours" in fields:
    data["properties"]["Estimated Hours"] = {
        "number": fields["Estimated Hours"]
    }
```

---

### 타입 3: Date (날짜)

**GitHub Projects:** Due Date (날짜)  
**Notion:** Due Date (Date)

⚠️ **주의:** Date는 GraphQL 쿼리 수정 필요!

#### 1. GraphQL 쿼리에 Date 타입 추가

```python
# sync_issues.py 약 86~120번 줄 부근
# get_issue_projects_info() 함수 내부

query = """
query($nodeId: ID!) {
  node(id: $nodeId) {
    ... on Issue {
      projectItems(first: 10) {
        nodes {
          fieldValues(first: 20) {
            nodes {
              # 기존 타입들...
              
              # ✨ Date 타입 추가
              ... on ProjectV2ItemFieldDateValue {
                date {
                  start
                  end
                }
                field {
                  ... on ProjectV2Field {
                    name
                  }
                }
              }
            }
          }
        }
      }
    }
  }
}
"""
```

#### 2. 파싱 로직 추가

```python
# _parse_projects_data() 함수 약 180~210번 줄 부근

for field_value in field_values:
    # 기존 타입들...
    
    # ✨ Date 추가
    elif "date" in field_value:
        field_obj = field_value.get("field", {})
        field_name = field_obj.get("name")
        date_value = field_value.get("date", {})
        field_data = date_value.get("start")  # start 날짜만 사용
    
    if field_name and field_data is not None:
        project_info["fields"][field_name] = field_data
```

#### 3. Notion에 추가

```python
# create_notion_page()와 update_notion_page()

if "Due Date" in fields:
    data["properties"]["Due Date"] = {
        "date": {
            "start": fields["Due Date"]  # ISO 8601 형식
        }
    }
```

---

### 타입 4: Text (텍스트)

**GitHub Projects:** Notes (텍스트)  
**Notion:** Notes (Text)

#### 코드 추가:

```python
# create_notion_page()와 update_notion_page()

if "Notes" in fields:
    data["properties"]["Notes"] = {
        "rich_text": [
            {
                "text": {
                    "content": str(fields["Notes"])[:2000]  # 2000자 제한
                }
            }
        ]
    }
```

---

### 타입 5: Iteration (스프린트/반복)

**이미 구현됨!** Sprint 필드 참고

```python
if "Sprint" in fields:
    data["properties"]["Sprint"] = {
        "rich_text": [
            {
                "text": {
                    "content": str(fields["Sprint"])
                }
            }
        ]
    }
```

---

## 🔧 실전 예시: 전체 과정

### 시나리오: Team과 Due Date 필드 추가

#### Step 1: Notion에 속성 추가

```
1. Team (Select)
   - Options: Frontend, Backend, Mobile, DevOps

2. Due Date (Date)
```

#### Step 2: GitHub Projects에 필드 추가

```
1. Team (Single select)
   - Options: Frontend, Backend, Mobile, DevOps

2. Due Date (Date)
```

#### Step 3: sync_issues.py 수정

##### 3-1. GraphQL 쿼리에 Date 추가 (Due Date용)

```python
# 줄 86~120 부근, get_issue_projects_info() 함수

fieldValues(first: 20) {
  nodes {
    # 기존 것들...
    
    # ✨ 추가
    ... on ProjectV2ItemFieldDateValue {
      date {
        start
        end
      }
      field {
        ... on ProjectV2Field {
          name
        }
      }
    }
  }
}
```

##### 3-2. 파싱 로직 추가

```python
# 줄 180~210 부근, _parse_projects_data() 함수

# ✨ Date 타입 추가
elif "date" in field_value:
    field_obj = field_value.get("field", {})
    field_name = field_obj.get("name")
    date_value = field_value.get("date", {})
    field_data = date_value.get("start")
```

##### 3-3. create_notion_page()에 추가

```python
# 줄 595~650 부근

# Projects 필드들
fields = projects_info.get("fields", {})

# ... 기존 필드들 ...

# ✨ Team 추가
if "Team" in fields:
    data["properties"]["Team"] = {
        "select": {
            "name": fields["Team"]
        }
    }

# ✨ Due Date 추가
if "Due Date" in fields:
    data["properties"]["Due Date"] = {
        "date": {
            "start": fields["Due Date"]
        }
    }
```

##### 3-4. update_notion_page()에도 동일하게 추가

```python
# 줄 730~780 부근

# ✨ Team 추가
if "Team" in fields:
    data["properties"]["Team"] = {
        "select": {
            "name": fields["Team"]
        }
    }

# ✨ Due Date 추가
if "Due Date" in fields:
    data["properties"]["Due Date"] = {
        "date": {
            "start": fields["Due Date"]
        }
    }
```

---

## 📋 필드 타입 매핑 참조표

| GitHub Projects 타입 | Notion 타입 | 코드 예시 |
|---------------------|-------------|----------|
| **Single select** | Select | `{"select": {"name": value}}` |
| **Number** | Number | `{"number": value}` |
| **Text** | Text | `{"rich_text": [{"text": {"content": value}}]}` |
| **Date** | Date | `{"date": {"start": value}}` |
| **Iteration** | Text | `{"rich_text": [{"text": {"content": str(value)}}]}` |

---

## 🎯 코드 수정 위치 요약

### 1. GraphQL 쿼리 (Date 타입만)

**파일:** `sync_issues.py`  
**함수:** `get_issue_projects_info()`  
**위치:** 약 86~120번 줄

Date/Iteration 같은 특수 타입만 추가 필요

### 2. 파싱 로직 (Date 타입만)

**파일:** `sync_issues.py`  
**함수:** `_parse_projects_data()`  
**위치:** 약 180~210번 줄

Date/Iteration 같은 특수 타입만 추가 필요

### 3. Notion 페이지 생성

**파일:** `sync_issues.py`  
**함수:** `create_notion_page()`  
**위치:** 약 595~650번 줄

**모든 커스텀 필드 추가 필요**

### 4. Notion 페이지 업데이트

**파일:** `sync_issues.py`  
**함수:** `update_notion_page()`  
**위치:** 약 730~780번 줄

**모든 커스텀 필드 추가 필요**

---

## 💡 복사-붙여넣기 템플릿

### Select 필드 템플릿

```python
# 필드 이름: "YOUR_FIELD_NAME"
# Notion 속성 이름: "YOUR_FIELD_NAME"

# create_notion_page()와 update_notion_page()에 추가:

if "YOUR_FIELD_NAME" in fields:
    data["properties"]["YOUR_FIELD_NAME"] = {
        "select": {
            "name": fields["YOUR_FIELD_NAME"]
        }
    }
```

### Number 필드 템플릿

```python
# 필드 이름: "YOUR_FIELD_NAME"

if "YOUR_FIELD_NAME" in fields:
    data["properties"]["YOUR_FIELD_NAME"] = {
        "number": fields["YOUR_FIELD_NAME"]
    }
```

### Text 필드 템플릿

```python
# 필드 이름: "YOUR_FIELD_NAME"

if "YOUR_FIELD_NAME" in fields:
    data["properties"]["YOUR_FIELD_NAME"] = {
        "rich_text": [
            {
                "text": {
                    "content": str(fields["YOUR_FIELD_NAME"])[:2000]
                }
            }
        ]
    }
```

### Date 필드 템플릿

#### 1. GraphQL 쿼리 추가 (이미 있으면 생략)

```python
# get_issue_projects_info() 함수 내부, fieldValues nodes에 추가:

... on ProjectV2ItemFieldDateValue {
  date {
    start
    end
  }
  field {
    ... on ProjectV2Field {
      name
    }
  }
}
```

#### 2. 파싱 추가 (이미 있으면 생략)

```python
# _parse_projects_data() 함수 내부:

elif "date" in field_value:
    field_obj = field_value.get("field", {})
    field_name = field_obj.get("name")
    date_value = field_value.get("date", {})
    field_data = date_value.get("start")
```

#### 3. Notion에 추가

```python
# create_notion_page()와 update_notion_page()에:

if "YOUR_DATE_FIELD" in fields:
    data["properties"]["YOUR_DATE_FIELD"] = {
        "date": {
            "start": fields["YOUR_DATE_FIELD"]
        }
    }
```

---

## 🎨 실전 예시 1: Team 필드 추가

### 1. Notion 설정

```
속성 추가:
Name: Team
Type: Select
Options: Frontend, Backend, Mobile, DevOps
```

### 2. GitHub Projects 설정

```
필드 추가:
Name: Team
Type: Single select
Options: Frontend, Backend, Mobile, DevOps
```

### 3. 코드 수정

#### sync_issues.py 열기

#### create_notion_page() 함수 찾기 (약 595번 줄)

**기존 코드:**
```python
# Priority
if "Priority" in fields:
    data["properties"]["Priority"] = {
        "select": {
            "name": fields["Priority"]
        }
    }

# Story Points (Number)
if "Story Points" in fields:
    ...
```

**추가:**
```python
# Priority
if "Priority" in fields:
    data["properties"]["Priority"] = {
        "select": {
            "name": fields["Priority"]
        }
    }

# ✨ Team 추가 (여기!)
if "Team" in fields:
    data["properties"]["Team"] = {
        "select": {
            "name": fields["Team"]
        }
    }

# Story Points (Number)
if "Story Points" in fields:
    ...
```

#### update_notion_page() 함수에도 동일하게 추가 (약 760번 줄)

**기존 코드:**
```python
# Priority
if "Priority" in fields:
    data["properties"]["Priority"] = {
        "select": {
            "name": fields["Priority"]
        }
    }

# Story Points (Number)
if "Story Points" in fields:
    ...
```

**추가:**
```python
# Priority
if "Priority" in fields:
    data["properties"]["Priority"] = {
        "select": {
            "name": fields["Priority"]
        }
    }

# ✨ Team 추가 (여기!)
if "Team" in fields:
    data["properties"]["Team"] = {
        "select": {
            "name": fields["Team"]
        }
    }

# Story Points (Number)
if "Story Points" in fields:
    ...
```

---

## 🎨 실전 예시 2: Estimated Hours + Due Date 추가

### 1. Notion 설정

```
1. Estimated Hours (Number)
2. Due Date (Date)
```

### 2. GitHub Projects 설정

```
1. Estimated Hours (Number)
2. Due Date (Date)
```

### 3. 코드 수정

#### GraphQL 쿼리에 Date 추가 (한 번만)

**위치:** `get_issue_projects_info()` 함수 약 86번 줄

```python
fieldValues(first: 20) {
  nodes {
    # ... 기존 타입들 ...
    
    # ✨ Date 타입 추가 (없으면)
    ... on ProjectV2ItemFieldDateValue {
      date {
        start
        end
      }
      field {
        ... on ProjectV2Field {
          name
        }
      }
    }
  }
}
```

#### 파싱에 Date 추가 (한 번만)

**위치:** `_parse_projects_data()` 함수 약 195번 줄

```python
# ✨ Date 추가 (없으면)
elif "date" in field_value:
    field_obj = field_value.get("field", {})
    field_name = field_obj.get("name")
    date_value = field_value.get("date", {})
    field_data = date_value.get("start")
```

#### Notion 필드 추가 (create & update 둘 다)

```python
# Estimated Hours (Number)
if "Estimated Hours" in fields:
    data["properties"]["Estimated Hours"] = {
        "number": fields["Estimated Hours"]
    }

# Due Date (Date)
if "Due Date" in fields:
    data["properties"]["Due Date"] = {
        "date": {
            "start": fields["Due Date"]
        }
    }
```

---

## 🎨 실전 예시 3: Multi-select (여러 개 선택)

**GitHub Projects:** Tags (Multi-select)  
**Notion:** Tags (Multi-select)

⚠️ **주의:** Multi-select는 복잡합니다!

#### GraphQL 쿼리 추가

```python
... on ProjectV2ItemFieldMultiSelectValue {
  values {
    name
  }
  field {
    ... on ProjectV2MultiSelectField {
      name
    }
  }
}
```

#### 파싱 추가

```python
elif "values" in field_value:  # Multi-select
    field_obj = field_value.get("field", {})
    field_name = field_obj.get("name")
    values = field_value.get("values", [])
    field_data = [v.get("name") for v in values]
```

#### Notion 추가

```python
if "Tags" in fields and isinstance(fields["Tags"], list):
    data["properties"]["Tags"] = {
        "multi_select": [
            {"name": tag} for tag in fields["Tags"]
        ]
    }
```

---

## 🧪 테스트

### 1. 코드 수정 완료 후

```bash
# Lint 확인 (선택사항)
python -m py_compile sync_issues.py

# Git 커밋
git add sync_issues.py
git commit -m "커스텀 필드 추가: Team, Due Date"
git push
```

### 2. GitHub Projects에서 필드 값 설정

```
Issue #1:
  - Team: Frontend
  - Due Date: 2024-02-01
```

### 3. Actions 실행

- Actions → Run workflow
- 또는 이슈 수정하여 자동 트리거

### 4. Notion 확인

```
Title: 테스트 이슈
Team: Frontend          ← 새 필드!
Due Date: 2024-02-01    ← 새 필드!
```

---

## ⚠️ 주의사항

### 필드 이름 정확히 일치

```
GitHub Projects: "Story Points"
Notion: "Story Points"
코드: if "Story Points" in fields

→ 대소문자, 공백 모두 정확히 일치해야 함!
```

### Notion Select 옵션 일치

```
GitHub Projects: "In progress"
Notion Select 옵션: "In progress"

→ 정확히 일치해야 함!
→ 안 맞으면 "validation failed" 에러
```

### 필드가 없을 때 처리

```python
# if 문 사용으로 자동 처리
if "Team" in fields:  # 없으면 추가 안 됨
    data["properties"]["Team"] = ...
```

필드가 없어도 에러 나지 않음!

---

## 🐛 문제 해결

### "validation failed" 에러

**원인:** Notion 속성 타입 불일치 또는 옵션 불일치

**해결:**
1. Notion 속성 타입 확인
2. Select 옵션이 정확히 일치하는지 확인
3. 대소문자, 공백 확인

### 커스텀 필드가 동기화 안 됨

**확인:**
1. GitHub Projects에 필드가 있고 값이 설정되어 있는지
2. Notion에 같은 이름의 속성이 있는지
3. 코드에 해당 필드 추가했는지
4. `if "필드이름"` 이름이 정확한지

### GraphQL 에러

**원인:** Date/Multi-select 같은 특수 타입 쿼리 누락

**해결:**
1. GraphQL 쿼리에 해당 타입 추가
2. 파싱 로직 추가
3. Notion 필드 추가

---

## 📚 더 알아보기

### Notion API 속성 타입:

- [Notion Property Objects](https://developers.notion.com/reference/property-object)
- [Notion Property Values](https://developers.notion.com/reference/property-value-object)

### GitHub GraphQL API:

- [ProjectV2 Types](https://docs.github.com/en/graphql/reference/objects#projectv2)
- [ProjectV2ItemFieldValue](https://docs.github.com/en/graphql/reference/interfaces#projectv2itemfieldvalue)

---

## 🎯 빠른 참고: 자주 사용하는 필드

### 팀/조직 관리
```python
# Team, Department, Squad 등
if "Team" in fields:
    data["properties"]["Team"] = {
        "select": {"name": fields["Team"]}
    }
```

### 시간/일정
```python
# Due Date, Start Date, Deadline 등
if "Due Date" in fields:
    data["properties"]["Due Date"] = {
        "date": {"start": fields["Due Date"]}
    }

# Estimated Hours, Time Spent 등
if "Estimated Hours" in fields:
    data["properties"]["Estimated Hours"] = {
        "number": fields["Estimated Hours"]
    }
```

### 워크플로우
```python
# Stage, Phase, Milestone 등
if "Stage" in fields:
    data["properties"]["Stage"] = {
        "select": {"name": fields["Stage"]}
    }
```

### 메모/노트
```python
# Notes, Description, Comments 등
if "Notes" in fields:
    data["properties"]["Notes"] = {
        "rich_text": [{"text": {"content": str(fields["Notes"])[:2000]}}]
    }
```

---

## ✅ 체크리스트

커스텀 필드 추가 시:

- [ ] Notion에 속성 추가
- [ ] GitHub Projects에 필드 추가
- [ ] 필드 타입 확인 (Select/Number/Date/Text)
- [ ] GraphQL 쿼리 수정 (Date 타입만)
- [ ] 파싱 로직 추가 (Date 타입만)
- [ ] `create_notion_page()`에 코드 추가
- [ ] `update_notion_page()`에 코드 추가
- [ ] 코드 테스트
- [ ] git commit & push
- [ ] Actions 실행
- [ ] Notion에서 확인

---

## 🎉 성공!

이제 원하는 모든 커스텀 필드를 추가할 수 있습니다!

팀의 워크플로우에 맞게 자유롭게 커스터마이징하세요! 🚀

---

## 💬 질문이 있나요?

- 특정 필드 타입 추가가 어려우신가요?
- 에러가 발생하나요?
- GitHub Issues에 질문을 남겨주세요!

