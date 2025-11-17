# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- Organization 공용 이슈 템플릿 설정 가이드 추가 (`docs/10-organization-issue-templates.md`)
  - `.github` 레포지토리 생성 방법
  - Form 템플릿 작성 가이드 (버그 리포트, 기능 제안, 질문)
  - Organization 전역 적용 방법
  - 동기화 스크립트와 연계 방법
  - 라벨 기반 템플릿 식별 코드 예시
  - 본문 파싱 Python 코드 예시
- 한글 이슈 템플릿 추가 (`.github/ISSUE_TEMPLATE/`)
  - `bug_report.md`: 🐛 버그 리포트 (한글, 범용적)
  - `feature_request.md`: ✨ 기능 제안 (한글)
  - `question.md`: ❓ 질문 (한글)
  - `config.yml`: 템플릿 설정 파일

### Changed
- README.md: 빠른 시작 가이드에 Organization 이슈 템플릿 가이드 추가
- README.md: 향후 계획에 이슈 템플릿 지원 항목 완료로 표시
- docs/README.md: Step 10으로 Organization 이슈 템플릿 가이드 추가
- `.github/ISSUE_TEMPLATE/bug_report.md`: 영어에서 한글로 전환, 범용적인 형태로 개선
  - 심각도 선택 추가 (Critical/High/Medium/Low)
  - 웹/모바일 환경 구분
  - 체크리스트 추가

### Documentation
- Organization 전체에서 사용 가능한 공용 이슈 템플릿 설정 방법 문서화
- 템플릿 우선순위 규칙 및 제한사항 설명
- 실전 활용 가능한 완전한 템플릿 예시 제공
- 예상 소요 시간: 30-45분

