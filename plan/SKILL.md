---
name: plan
description: PM 기획 슈퍼파워. PRD 작성 / spec→plan 변환 / 와이어프레임 명세 / 대시보드 요구사항 + 개발 직전 HTML+PDF 기획서 생성. /pm-kit 으로 실행.
---

# pm-kit — PM 기획 슈퍼파워

wooksDev 기획 파이프라인(planner → back/front → verifier)에 최적화된 통합 기획 스킬.

## 실행 방법

`/pm-kit` 입력 시 아래 모드 중 하나를 선택하게 한다.

    1. PRD       — 요구사항 대화 수집 → prd.md 생성
    2. PLAN      — spec.md 읽기 → plan.md 자동 생성
    3. WIREFRAME — 화면 와이어프레임 명세 작성
    4. DASHBOARD — 질문 기반 대시보드 요구사항 문서

인자를 직접 넘기면 해당 모드로 바로 진입한다.
예: `/pm-kit prd`, `/pm-kit plan`, `/pm-kit wireframe`, `/pm-kit dashboard`

---

## MODE 1: PRD — 제품 요구사항 문서

**Step 1 — 컨텍스트 수집 (대화)**
아래 항목을 대화로 파악한다. 한 번에 다 묻지 말고 자연스럽게 이어간다.
- 기능/제품명, 해결하려는 문제, 타겟 사용자
- 핵심 기능 (3~5개), 범위 밖 (non-goal)
- 성공 지표 (KPI), 마감/마일스톤

**Step 2 — PRD 초안 작성**

아래 구조로 작성한다:

    # [기능명] PRD
    **상태**: Draft | **날짜**: YYYY-MM-DD | **작성자**: -

    ## 요약 (3줄)
    ## 문제 정의
    ## 목표 / 비목표
    | 목표 | 비목표 |
    |------|--------|
    ## 사용자 스토리
    - As a [사용자], I want to [행동], so that [목적]
    ## 기능 범위
    ## 성공 지표 (KPI)
    ## 마일스톤
    | 단계 | 내용 | 기한 |
    |------|------|------|
    ## 미결 질문
    - [ ] ...

**Step 3 — 품질 점검 (100점 척도)**

| 항목 | 배점 |
|------|------|
| 문제 정의 명확성 | 20 |
| 목표/비목표 구분 | 15 |
| 사용자 스토리 완성도 | 20 |
| KPI 측정 가능성 | 20 |
| 마일스톤 현실성 | 15 |
| 미결 질문 정리 | 10 |

90점 미만이면 부족한 항목 보완 후 저장.

**Step 4 — 저장**
- 경로: `docs/{feature-name}-prd.md`
- wooksDev 프로젝트 내라면: `projects/{프로젝트명}/.claude/spec.md` 동시 저장 여부 확인

---

## MODE 2: PLAN — spec.md → plan.md 변환

**Step 1 — spec.md 탐색**
순서대로 탐색: `projects/{프로젝트명}/.claude/spec.md` → `docs/spec.md` → 현재 디렉토리 `spec.md`
없으면 사용자에게 경로 질문.

**Step 2 — 명세 분석**
spec.md에서 기능 목록/AC, 기술 스택, `[NEEDS CLARIFICATION]` 태그 추출.
태그 발견 시 사용자에게 먼저 확인 후 진행.

**Step 3 — plan.md 작성**

아래 구조로 작성한다:

    # [기능명] 구현 계획
    **기반 spec**: {경로} | **날짜**: YYYY-MM-DD

    ## 구현 순서
    ## 백엔드 태스크
    | # | 태스크 | 파일/경로 | 예상 시간 |
    |---|--------|----------|----------|
    ## 프론트엔드 태스크
    | # | 태스크 | 파일/경로 | 예상 시간 |
    |---|--------|----------|----------|
    ## 인터페이스 정의 (API / Props)
    ## 테스트 체크리스트 (verifier 기준)
    - [ ] AC 1: ...
    ## 위험 요소 / 주의사항

**Step 4 — 저장**
- spec.md 와 같은 폴더에 `plan.md` 저장
- `projects/{프로젝트명}/.claude/status/active-plan.md` 업데이트 여부 확인

---

## MODE 3: WIREFRAME — 화면 와이어프레임 명세

**Step 1 — 화면 정보 수집**
화면명, 핵심 플로우, 디바이스 타겟, 충실도(low / mid / high) 선택.

**Step 2 — 충실도별 출력**

Low-fi (구조): ASCII 박스 레이아웃으로 전체 섹션 배치 표현
Mid-fi (컴포넌트): Low-fi + 컴포넌트 명세 테이블 | 컴포넌트 | 타입 | 상태 | 데이터 |
High-fi (개발 직전): Mid-fi + px/rem 수치 + 인터랙션 노트 + 접근성 주석

**Step 3 — 인터랙션 & 접근성 주석**
클릭/호버/포커스 동작, 로딩/에러/빈 상태, aria-label, 키보드 내비게이션

**Step 4 — 반응형 정의**
| 브레이크포인트 | 레이아웃 변화 |
|--------------|-------------|
| mobile (< 768px) | ... |
| tablet (768~1024px) | ... |
| desktop (> 1024px) | ... |

**Step 5 — 저장**
경로: `docs/wireframes/{screen-name}-wireframe.md`

---

## MODE 4: DASHBOARD — 대시보드 요구사항

**Step 1 — 핵심 질문 수집**
누가 보는가, 어떤 결정을 위해 보는가, 현재 어디서 이 정보를 얻는가.

**Step 2 — 질문 → 지표 매핑**
| 비즈니스 질문 | 지표 | 차트 타입 | 갱신 주기 |
|-------------|------|---------|---------|

**Step 3 — 요구사항 문서 작성**

아래 구조로 작성한다:

    # [대시보드명] 요구사항
    **날짜**: YYYY-MM-DD

    ## 목적
    ## 핵심 질문 목록
    ## 지표 정의
    | 지표명 | 계산 방식 | 데이터 소스 | 갱신 주기 |
    ## 화면 구성
    ## 필터 / 드릴다운
    ## 접근 권한
    ## 미결 질문
    - [ ] ...

**Step 4 — 저장**
경로: `docs/{dashboard-name}-requirements.md`

---

## 기획 완료 → 개발 전 HTML 생성

모든 기획 문서 작업이 끝나면 반드시 아래 질문을 한 번 한다:

> "기획이 완료됐습니다. 개발에 들어가기 전에 전체 기획 내용을 HTML 파일로 정리할까요?
> (웹 브라우저에서 바로 보고, PDF로도 저장할 수 있습니다)"

사용자가 동의하면 아래 HTML 생성 워크플로우를 실행한다.

### HTML 생성 워크플로우

**Step 1 — 기획 문서 수집**
작업한 모든 md 파일 수집 (prd.md, spec.md, plan.md, wireframe.md 등)

**Step 2 — 디자인 원칙**
- 배경: 순백색 (#ffffff), 글자: 거의 검정 (#111111)
- 강조/구분선: 회색 계열 (#e5e5e5, #999999, #555555)
- 유채색 일체 금지 — 모노톤만 사용
- 폰트: 'Pretendard', 'Apple SD Gothic Neo', sans-serif
- 본문: line-height 1.8, font-size 16px, max-width 860px 중앙 정렬
- PDF 다운로드 버튼: 우측 상단 fixed 고정, 클릭 시 window.print()
- @media print: 버튼 숨김, @page margin 20mm, section page-break-inside avoid

**Step 3 — HTML 구성 요소 및 PDF 버튼**

PDF 버튼 (body 최상단 삽입):

    <button class="pdf-btn" onclick="window.print()">PDF로 저장</button>

PDF 버튼 스타일:

    .pdf-btn {
      position: fixed; top: 24px; right: 24px;
      background: #111111; color: #ffffff;
      border: none; border-radius: 4px;
      padding: 10px 20px; font-size: 14px; font-weight: 600;
      cursor: pointer; z-index: 100; transition: background 0.15s;
    }
    .pdf-btn:hover { background: #333333; }
    @media print {
      .pdf-btn { display: none; }
      @page { margin: 20mm; }
      body { padding: 0; }
      section { page-break-inside: avoid; }
    }

그 외 구성 요소:
- 문서 헤더 (제목, 상태 배지, 날짜)
- 섹션별 h2/h3 계층 (h2는 왼쪽 3px 검정 선 강조)
- 테이블 (줄무늬, 회색 테두리)
- 체크리스트 (input[type="checkbox"] 인터랙티브)
- 마일스톤 시각화 바
- 미결 질문 카드 (왼쪽 회색 선 강조)

**Step 4 — 저장 경로**
`reports/planning/YYYY-MM-DD_{기획명}_report.html`

---

## 공통 규칙

- 모든 기획 산출물은 .md 형식이 기본 (에이전트가 읽는 파일)
- HTML은 모든 기획 완료 후 사용자에게 한 번만 물어보고 생성
- wooksDev 프로젝트 내 작업 시 projects/{프로젝트명}/ 밖으로 나가지 않음
- spec.md 생성 완료 = planner 역할 완료 → back/front 에이전트로 넘어갈 준비
