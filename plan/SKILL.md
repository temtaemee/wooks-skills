---
name: agent-ops
description: 여러 AI 에이전트를 번갈아 쓰는 프로젝트를 위한 공통 운영 스킬. 루트 README 기반 공통 지침, .agent 작업 상태 폴더, cavecrew/caveman 세션 규칙, 에이전트별 부트로더, PRD/PLAN/WIREFRAME/DASHBOARD 기획 산출물과 HTML 리포트 워크플로우를 설계한다. /agent-kit 으로 실행.
---

# agent-ops — AI 에이전트 공통 운영 스킬

여러 AI 에이전트(Codex, Claude, Gemini, Cursor 등)를 번갈아 쓰는 프로젝트에서 공통 지침, 작업 상태, 핸드오프, 기획 산출물, 검증 흐름을 한 구조로 관리하는 통합 운영 스킬.

기획 기능(PRD, PLAN, WIREFRAME, DASHBOARD)은 이 스킬 안에 포함된 작업 유형 중 하나이며, 핵심 목적은 에이전트 운영 구조를 표준화하는 것이다.

## 실행 방법

`/agent-kit` 입력 시 아래 모드 중 하나를 선택하게 한다.

    1. AGENT     — 여러 AI가 공유하는 README + .agent 중심 에이전트 운영 구조 생성
    2. PRD       — 요구사항 대화 수집 → prd.md 생성
    3. PLAN      — spec.md 읽기 → plan.md 자동 생성
    4. WIREFRAME — 화면 와이어프레임 명세 작성
    5. DASHBOARD — 질문 기반 대시보드 요구사항 문서

인자를 직접 넘기면 해당 모드로 바로 진입한다.
예: `/agent-kit agent`, `/agent-kit prd`, `/agent-kit plan`, `/agent-kit wireframe`, `/agent-kit dashboard`

기존 호출 호환을 위해 `/pm-kit`은 기획 관련 모드(PRD, PLAN, WIREFRAME, DASHBOARD)의 별칭으로 취급할 수 있다. 단, 새 문서와 새 지침에는 기본 명령을 `/agent-kit`으로 적는다.

---

## MODE 1: AGENT — 공통 에이전트 운영 구조

여러 AI 에이전트(Codex, Claude, Gemini, Cursor 등)를 번갈아 사용할 때, 각 에이전트별 지침 파일과 작업 폴더에 내용이 흩어지지 않도록 저장소 루트의 `README.md`와 `.agent/`를 중심으로 운영 구조를 만든다.

**핵심 원칙**
- 긴 운영 지침은 `README.md` 한 곳에만 둔다.
- 에이전트가 읽고 쓰는 작업 상태, 계획, 핸드오프 문서는 `.agent/` 한 곳에 모은다.
- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` 등 에이전트별 지침 파일은 README를 읽으라는 짧은 부트로더 역할만 한다.
- `.agent/` 아래에는 별도 README를 두지 않는다. `.agent/` 사용 규칙도 루트 `README.md`에서 관리한다.
- `.claude/`, `.codex/`, `.gemini/` 등 에이전트별 폴더는 도구가 강제로 요구하는 최소 설정에만 사용하고, 프로젝트 지식과 작업 기록은 중복 저장하지 않는다.
- 공통 지침을 수정할 때는 README만 수정하고, 공통 작업 상태를 수정할 때는 `.agent/`만 수정한다.
- 에이전트가 작업을 시작하면 먼저 README를 읽고, 그 다음 `.agent/`와 관련 파일을 탐색하게 한다.
- 모든 에이전트는 `cavecrew`를 설치/활성화한 상태를 기본 전제로 삼고, 세션 시작 시 caveman 언어 모드로 동작한다.

**역할 분리**
- `README.md`: 모든 에이전트가 반드시 읽고 그대로 따라야 하는 상세 운영 지침.
- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`: 저장소 루트에 직접 두는 파일이다. 각 에이전트에게 README를 읽고 그대로 따르라고 지시하는 짧은 진입점이다.
- `.agent/`: 에이전트들이 작업 중 필요로 하는 맥락, 계획, 결정, 진행 상태, 핸드오프를 읽고 쓰는 공용 데이터 폴더. 이 안에는 README를 두지 않는다.
- `.claude/`, `.codex/`, `.gemini/`: 도구별 필수 설정만 두는 예외 공간. 공통 지침이나 작업 상태를 관리하지 않는다.
- `cavecrew`: 모든 에이전트가 세션 시작 시 활성화해야 하는 caveman-style 압축 실행 도구.

**에이전트 지침 파일 위치 규칙**
- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`는 반드시 저장소 루트에 파일로 만든다.
- `AGENTS.md/`, `CLAUDE.md/`, `GEMINI.md/`, `agent.md/`, `claude.md/`처럼 같은 이름의 폴더를 만들지 않는다.
- `agents/AGENTS.md`, `claude/CLAUDE.md`, `.agent/AGENTS.md`처럼 하위 폴더에 넣지 않는다.
- 파일명은 도구가 자동 인식하는 정확한 대문자 파일명을 우선 사용한다: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`.
- 예외는 도구가 공식적으로 폴더 위치를 요구하는 경우뿐이다. 예: Cursor는 `.cursor/rules/`를 쓸 수 있다.
- 이미 잘못 만들어진 지침 폴더가 있으면 내용을 확인한 뒤 루트 지침 파일로 이관하고, 폴더 삭제 여부는 사용자에게 확인한다.

**부트로더 직접 삽입 예외**
- 에이전트별 지침 파일은 기본적으로 README를 가리키는 짧은 부트로더로 유지한다.
- 단, 에이전트가 자주 놓치면 안 되는 필수 종료 루틴은 에이전트별 지침 파일에 직접 넣는다.
- 필수 직접 삽입 항목:
  - README를 읽고 그대로 따르기
  - `cavecrew` 설치/활성화 및 caveman 언어 모드 시작
  - 기획 문서 작업 완료 후 사용자에게 HTML 리포트 생성 여부 묻기
- HTML 리포트 생성 질문은 README에도 넣고, `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` 같은 에이전트 지침 파일에도 직접 넣는다.

**에이전트 실행 규칙**
- 작업 시작 시 에이전트는 자기 전용 지침 파일(`AGENTS.md`, `CLAUDE.md`, `GEMINI.md` 등)을 통해 `README.md`를 읽고, README의 상세 지침을 그대로 따른다.
- 에이전트 지침 파일을 만들 때는 저장소 루트에 정확한 파일명으로 생성한다. 지침 파일용 폴더를 새로 만들지 않는다.
- 작업 중 필요한 프로젝트 맥락, 현재 계획, 결정 기록, 진행 상태, 핸드오프 정보는 `.agent/`에서 읽는다.
- 작업 유형이 명확하면 `.agent/` 아래에 작업별 폴더를 만들고 그 안에서 관련 파일을 관리한다. 예: 기획서 작업은 `.agent/planning/` 또는 `.agent/기획서/`.
- 각 작업별 폴더는 과거, 현재, 미래 정보를 섞지 않도록 `active/`, `cancle/`, `legacy/`, `plan/` 하위 폴더로 상태를 나누어 관리한다.
- 작업 중 새로 생긴 계획, 결정, 상태 변화, 다음 에이전트가 알아야 할 내용은 `.agent/`에 쓴다.
- 특정 작업 폴더에서 내용을 읽고 이해해야 하거나 수정 후 다시 반영해야 한다면, README에 그 경로와 갱신 규칙을 명시한다.
- README에는 장기적으로 유지할 운영 지침만 쓴다. 진행 중인 작업 상태나 임시 메모는 README에 쓰지 않는다.
- `.agent/`에는 에이전트가 이어받아야 할 작업 정보를 쓴다. 긴 운영 지침은 `.agent/`에 중복하지 않는다.
- `.agent/` 하위 폴더 사용법은 `.agent/README.md`가 아니라 루트 `README.md`의 `.agent Folder Rules`에서 관리한다.
- 에이전트별 폴더에는 공통 지침, 공통 맥락, 진행 상태를 저장하지 않는다. 필요한 경우 README 또는 `.agent/`로 옮긴다.
- 세션 시작 시 `cavecrew` 설치/활성화 여부를 확인한다. 없으면 가능한 환경에서는 설치하고, 설치할 수 없으면 `.agent/handoff.md`에 blocker로 남긴다.
- 기본 응답과 내부 작업 기록은 caveman 언어 모드로 짧고 압축적으로 작성한다.
- 보안 경고, 삭제/되돌리기 같은 위험 작업 확인, 사용자가 오해할 수 있는 내용은 일반 문장으로 명확하게 쓴 뒤 caveman 모드로 돌아간다.
- PRD, spec, plan, wireframe, dashboard 등 기획 문서 작업이 끝나면 반드시 사용자에게 HTML 리포트 생성 여부를 묻는다. 사용자가 동의하거나 요청하면 HTML 리포트를 생성한다.

**권장 파일 구조**

    /
    ├── README.md              # 모든 AI가 공통으로 읽는 메인 지침
    ├── AGENTS.md              # Codex/GPT 계열 부트로더
    ├── CLAUDE.md              # Claude 부트로더
    ├── GEMINI.md              # Gemini 부트로더
    ├── .agent/                # 모든 AI가 공통으로 읽고 쓰는 작업 상태 폴더
    │   ├── context.md         # 프로젝트/도메인 공통 맥락
    │   ├── active-plan.md     # 현재 진행 중인 계획
    │   ├── decisions.md       # 결정 로그
    │   ├── handoff.md         # 다음 에이전트에게 넘길 상태
    │   ├── planning/          # 기획서/PRD/spec/plan 작업 파일
    │   │   ├── active/        # 현재 실행 중인 기획 작업
    │   │   ├── cancle/        # 취소된 기획 작업
    │   │   ├── legacy/        # 완료된 기획 작업
    │   │   └── plan/          # 앞으로 진행할 기획 작업
    │   ├── implementation/    # 구현 작업 파일
    │   ├── verification/      # 검증 작업 파일
    │   └── tasks/             # 작업별 세부 계획과 결과
    ├── .cursor/rules/         # Cursor 사용 시 부트로더 규칙
    └── ...

**에이전트별 부트로더 템플릿**

    # Agent Instructions

    Before doing any work in this repository, read `README.md` and follow it
    exactly.

    Ensure `cavecrew` is installed and active. Start every session in caveman
    language mode unless `README.md` says otherwise.

    After completing planning documents such as PRD, spec, plan, wireframe, or
    dashboard requirements, ask the user whether to generate an HTML report.
    If the user agrees or requests it, generate the HTML report.

    `README.md` is the source of truth for:
    - project purpose
    - agent workflow
    - file conventions
    - planning rules
    - output format
    - handoff rules

    Use `.agent/` for shared agent-written context, plans, decisions, task notes,
    and handoff state. Do not duplicate project knowledge across `.claude/`,
    `.codex/`, `.gemini/`, or other tool-specific folders unless the tool
    requires it.

**README.md 권장 구조**

    # [프로젝트명] Agent Guide

    ## Purpose
    이 저장소의 목적과 에이전트가 수행해야 할 역할을 설명한다.

    ## Source Of Truth
    모든 에이전트는 작업 시작 전에 이 README를 먼저 읽는다.
    에이전트는 이 README의 상세 지침을 그대로 따른다.
    에이전트별 지침 파일은 README로 연결하는 부트로더 역할만 한다.
    `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`는 저장소 루트에 직접 둔다.
    에이전트 지침 파일을 넣기 위한 별도 폴더는 만들지 않는다.

    ## Shared Agent Workspace
    모든 에이전트가 읽고 쓰는 작업 상태는 `.agent/`에 둔다.
    `.agent/` 아래에는 README를 두지 않는다. `.agent/` 사용 규칙도 이 루트 README에서 관리한다.
    `.claude/`, `.codex/`, `.gemini/` 같은 도구별 폴더에는 공통 프로젝트 지식을 중복 저장하지 않는다.
    에이전트가 동작 중 필요로 하는 맥락, 계획, 결정, 진행 상태, 핸드오프는 `.agent/`에서 읽고 쓴다.

    ## Cavecrew Rules
    모든 에이전트는 세션 시작 시 `cavecrew` 설치/활성화 여부를 확인한다.
    `cavecrew`가 없으면 설치 가능한 환경에서는 설치하고, 불가능하면 `.agent/handoff.md`에 blocker로 남긴다.
    세션 기본 언어는 caveman 언어 모드다.
    기본 응답, 조사 결과, 작업 기록은 짧고 압축적으로 쓴다.
    보안 경고, 위험 작업 확인, 되돌릴 수 없는 작업, 사용자가 오해할 수 있는 내용은 일반 문장으로 명확히 쓴다.
    코드 위치 조사, 작은 수정, diff 리뷰를 위임할 때는 `cavecrew-investigator`, `cavecrew-builder`, `cavecrew-reviewer`를 우선 사용한다.

    ## Planning HTML Report Rule
    PRD, spec, plan, wireframe, dashboard 요구사항 등 기획 문서 작업이 끝나면 반드시 사용자에게 HTML 리포트 생성 여부를 묻는다.
    사용자가 동의하거나 요청하면 작업한 기획 문서를 수집해 HTML 파일로 정리한다.
    HTML 리포트는 브라우저에서 바로 볼 수 있고 PDF로 저장할 수 있어야 한다.
    이 규칙은 에이전트가 놓치지 않도록 `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` 같은 에이전트 지침 파일에도 직접 넣는다.

    ## Runtime Memory Rules
    작업 시작: README를 읽고 그대로 따른다.
    작업 중 읽기: 필요한 맥락, 계획, 결정, 상태, 핸드오프는 `.agent/`에서 확인한다.
    작업 중 쓰기: 새 계획, 결정, 상태 변화, 다음 에이전트가 알아야 할 내용은 `.agent/`에 기록한다.
    작업별 관리: 기획서 작업은 `.agent/planning/`에서 읽고 쓰며, 수정 사항은 해당 폴더의 관련 파일에 다시 반영한다.
    상태별 관리: 실행 중인 내용은 `active/`, 취소된 내용은 `cancle/`, 완료된 내용은 `legacy/`, 앞으로의 계획은 `plan/`에서 관리한다.
    README에는 장기 운영 지침만 둔다.
    `.agent/`에는 실행 중 필요한 공유 작업 정보를 둔다.

    ## Repository Map
    주요 폴더와 파일의 역할을 정리한다.

    ## Agent Workflow
    1. 요청 의도 파악
    2. README와 관련 파일 확인
    3. 필요한 산출물 생성 또는 수정
    4. 결과 검증
    5. 변경사항 요약

    ## File Conventions
    문서, 스펙, 계획, 리포트, 소스 파일의 저장 위치와 네이밍 규칙을 정한다.

    ## Planning Rules
    PRD, spec, plan, wireframe, dashboard 요구사항 작성 방식을 정한다.

    ## Handoff Rules
    planner, back, front, verifier 등 다음 에이전트에게 넘길 산출물과 완료 조건을 정한다.

    ## .agent Folder Rules
    `.agent/context.md`에는 오래 유지되는 프로젝트 맥락을 기록한다.
    `.agent/active-plan.md`에는 현재 작업 계획만 기록한다.
    `.agent/decisions.md`에는 되돌아봐야 할 결정과 이유를 기록한다.
    `.agent/handoff.md`에는 다음 에이전트가 바로 이어받을 상태를 기록한다.
    `.agent/planning/`에는 PRD, spec, plan, wireframe, dashboard 요구사항 등 기획서 작업 파일을 기록한다.
    README에는 에이전트가 `.agent/planning/`을 읽고 기획 내용을 파악한 뒤, 수정되는 내용이 있으면 같은 폴더의 관련 파일에 다시 반영하라는 지침을 둔다.
    `.agent/README.md`나 `.agent/planning/README.md`는 만들지 않는다. 하위 폴더 사용 규칙은 루트 README에 쓴다.
    각 작업별 폴더는 `active/`, `cancle/`, `legacy/`, `plan/` 하위 폴더를 가진다.
    현재 실행 중인 작업은 `active/`, 취소된 작업은 `cancle/`, 완료된 작업은 `legacy/`, 앞으로 할 계획은 `plan/`에 둔다.
    실행 중 새로 알게 된 내용 중 다음 에이전트도 알아야 하는 내용은 `.agent/`에 반영한다.

    ## Output Format
    최종 응답, 문서 형식, 체크리스트, 검증 결과 표기 방식을 정한다.

**Step 1 — 기존 지침 파일 탐색**
루트에서 아래 파일을 확인한다.
- `README.md`
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`
- 잘못 생성된 지침 폴더: `AGENTS.md/`, `CLAUDE.md/`, `GEMINI.md/`, `agent.md/`, `claude.md/`, `agents/`, `claude/`
- `.cursor/rules/*`
- `.agent/*`
- `.claude/*`
- `.codex/*`
- `.gemini/*`
- 기타 에이전트 지침 파일

이미 긴 지침이나 작업 상태가 여러 파일에 중복되어 있으면 내용을 비교해 README로 옮길 공통 지침과 `.agent/`로 옮길 작업 상태를 구분한다.

**Step 2 — README + .agent 중심 구조 제안**
사용자에게 아래 방향을 짧게 확인한다.
- `README.md`를 공통 지침의 단일 원천으로 둘지
- `.agent/`를 공통 에이전트 작업 폴더로 둘지
- 에이전트별 부트로더 파일을 어떤 종류까지 만들지
- 기존 긴 지침은 README로, 기존 작업 상태와 핸드오프 문서는 `.agent/`로 이관할지

**Step 3 — README 작성 또는 갱신**
README에는 에이전트가 실제로 따라야 할 공통 운영 지침을 넣는다.
최소 포함 항목:
- 프로젝트 목적
- source of truth 원칙
- README 지침을 그대로 따르라는 규칙
- `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`를 루트 파일로 만들고 하위 폴더에 넣지 않는 규칙
- `.agent/`를 공통 읽기/쓰기 작업 공간으로 쓰는 규칙
- `.agent/` 하위 README를 만들지 않고 루트 README에서 폴더 규칙을 관리하는 규칙
- 모든 에이전트의 `cavecrew` 설치/활성화 규칙
- 세션 시작 시 caveman 언어 모드로 동작하는 규칙
- 기획 문서 완료 후 HTML 리포트 생성 여부를 묻는 규칙
- 위 HTML 리포트 규칙을 에이전트 지침 파일에 직접 삽입하는 규칙
- README와 `.agent/`의 역할 분리
- 실행 중 읽기/쓰기 규칙
- 작업 유형별 `.agent/{작업폴더}/` 사용 규칙
- 작업별 폴더 내부의 `active/`, `cancle/`, `legacy/`, `plan/` 상태 관리 규칙
- 기획서 작업 시 `.agent/planning/`에서 읽고 쓰며 변경사항을 다시 반영하라는 규칙
- 저장소 구조
- 공통 작업 흐름
- 파일/문서 네이밍 규칙
- 계획/구현/검증 규칙
- 에이전트 간 handoff 규칙
- 최종 응답 형식

**Step 4 — 부트로더 파일 작성 또는 정리**
각 에이전트별 파일은 저장소 루트에 직접 만들고, README를 읽으라는 짧은 지침만 남긴다.
긴 지침, 중복 규칙, 프로젝트 정책은 부트로더에 넣지 않는다.
에이전트 지침 파일을 담기 위한 폴더는 만들지 않는다.

권장 `AGENTS.md`:

    # AGENTS.md

    Before doing any work in this repository, read `README.md` and follow it
    exactly.

    Ensure `cavecrew` is installed and active. Start the session in caveman
    language mode.

    After completing planning documents such as PRD, spec, plan, wireframe, or
    dashboard requirements, ask the user whether to generate an HTML report.
    If the user agrees or requests it, generate the HTML report.

    `README.md` is the source of truth for this repository's agent workflow,
    file conventions, planning rules, validation rules, and handoff process.

    Use `.agent/` for shared agent-written context, plans, decisions, task notes,
    and handoff state.

권장 `CLAUDE.md`:

    # CLAUDE.md

    Before doing any work in this repository, read `README.md` and follow it
    exactly.

    Ensure `cavecrew` is installed and active. Start the session in caveman
    language mode.

    After completing planning documents such as PRD, spec, plan, wireframe, or
    dashboard requirements, ask the user whether to generate an HTML report.
    If the user agrees or requests it, generate the HTML report.

    Keep this file as a short pointer only. Do not duplicate long project
    instructions or shared task state here. Use `.agent/` for shared agent notes.

권장 `GEMINI.md`:

    # GEMINI.md

    Before doing any work in this repository, read `README.md` and follow it
    exactly.

    Ensure `cavecrew` is installed and active. Start the session in caveman
    language mode.

    After completing planning documents such as PRD, spec, plan, wireframe, or
    dashboard requirements, ask the user whether to generate an HTML report.
    If the user agrees or requests it, generate the HTML report.

    Treat `README.md` as the shared instruction source for all AI agents.
    Use `.agent/` for shared context, plans, decisions, and handoff notes.

**Step 5 — .agent 폴더 작성 또는 정리**
`.agent/`가 없으면 생성하고, 있으면 현재 내용을 보존하면서 아래 기본 구조에 맞춘다.

`.agent/` 아래에는 README를 만들지 않는다.
`.agent/` 폴더 사용 규칙, 작업별 폴더 규칙, 상태 이동 규칙은 루트 `README.md`에 적는다.

권장 `.agent/context.md`:

    # Shared Context

    Long-lived project facts, domain notes, architecture assumptions, and
    conventions that future agents should know.

권장 `.agent/active-plan.md`:

    # Active Plan

    Current plan, status, blockers, and next actions.

권장 `.agent/decisions.md`:

    # Decisions

    Record important decisions with date, context, decision, and reason.

권장 `.agent/handoff.md`:

    # Handoff

    Current state for the next agent:
    - What changed
    - What remains
    - Verification performed
    - Known risks or blockers

권장 `.agent/planning/` 파일:

    .agent/planning/
    ├── active/                # 현재 실행 중인 기획 작업
    │   └── {작업명}/
    │       ├── brief.md       # 초기 요청과 배경
    │       ├── prd.md         # 제품 요구사항 문서
    │       ├── spec.md        # 개발 명세
    │       ├── plan.md        # 구현 계획
    │       ├── wireframe.md   # 화면 명세
    │       ├── dashboard.md   # 대시보드 요구사항
    │       └── changelog.md   # 기획 변경 기록
    ├── cancle/                # 취소된 기획 작업
    ├── legacy/                # 완료된 기획 작업
    └── plan/                  # 앞으로 진행할 기획 작업

**작업별 폴더 처리 규칙**
- 기획서 작업이면 `.agent/planning/`을 먼저 확인하고, 없으면 만든다.
- README에는 기획 관련 작업 시 `.agent/planning/`을 읽어 맥락을 파악하고, 수정되는 내용은 다시 `.agent/planning/`에 반영하라고 명시한다.
- 실제 최종 산출물이 `docs/`나 `projects/{프로젝트명}/.claude/spec.md` 같은 다른 경로에 저장되더라도, 에이전트 간 이어받기가 필요한 작업 맥락과 변경 기록은 `.agent/planning/`에도 남긴다.
- 구현 작업은 `.agent/implementation/`, 검증 작업은 `.agent/verification/`처럼 같은 패턴으로 확장한다.

**작업 상태 이동 규칙**
- 새로 예정된 작업은 해당 작업별 폴더의 `plan/{작업명}/`에 만든다.
- 사용자가 실행을 승인하거나 에이전트가 실제 작업을 시작하면 `plan/{작업명}/`을 `active/{작업명}/`으로 이동한다.
- 작업이 취소되면 `active/{작업명}/` 또는 `plan/{작업명}/`을 `cancle/{작업명}/`으로 이동하고 취소 사유를 남긴다.
- 작업이 완료되면 `active/{작업명}/`을 `legacy/{작업명}/`으로 이동하고 최종 결과, 변경 요약, 검증 결과를 남긴다.
- 다음 에이전트는 먼저 `active/`를 확인해 현재 진행 중인 작업을 파악하고, 필요하면 `plan/`, `legacy/`, `cancle/` 순서로 맥락을 확인한다.
- 상태 이동 시 루트 README의 경로 지침이 달라져야 하면 함께 갱신한다.

**도구별 폴더 처리 규칙**
- `.claude/`, `.codex/`, `.gemini/`는 각 도구가 실제로 요구하는 설정 파일만 둔다.
- 공통 프로젝트 지식, 계획, 진행 상태, 핸드오프는 `.agent/`로 옮긴다.
- 도구별 폴더를 삭제하거나 크게 바꾸기 전에는 사용자에게 확인한다.
- 이미 중요한 파일이 있으면 먼저 내용을 읽고, README 또는 `.agent/`로 이관한 뒤 중복 여부를 점검한다.

**Step 6 — 정합성 점검**
- README에 실제 공통 지침이 들어 있는가
- 에이전트별 파일이 README를 명확히 가리키는가
- 에이전트별 파일과 README에 `cavecrew` 설치/활성화 및 caveman 언어 모드 규칙이 들어 있는가
- `.agent/`에 공통 작업 상태와 핸드오프 문서가 모여 있는가
- `.claude/`, `.codex/`, `.gemini/`에 공통 지식이 중복 저장되어 있지 않은가
- 같은 규칙이 여러 파일에 중복되어 있지 않은가
- 다음 에이전트가 README와 `.agent/`만 읽어도 작업 흐름과 현재 상태를 이해할 수 있는가

**완료 조건**
- `README.md`가 공통 지침의 source of truth가 된다.
- `.agent/`가 모든 AI 에이전트의 공통 읽기/쓰기 작업 폴더가 된다.
- 모든 에이전트가 `cavecrew` 설치/활성화 확인 후 caveman 언어 모드로 세션을 시작하도록 지침화된다.
- 에이전트별 지침 파일은 짧은 부트로더로 정리된다.
- 중복된 긴 지침은 제거되거나 README로 이관된다.
- 중복된 작업 상태, 계획, 핸드오프 문서는 제거되거나 `.agent/`로 이관된다.
- 사용자가 여러 AI를 번갈아 써도 같은 기준으로 작업할 수 있다.

---

## MODE 2: PRD — 제품 요구사항 문서

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

## MODE 3: PLAN — spec.md → plan.md 변환

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

## MODE 4: WIREFRAME — 화면 와이어프레임 명세

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

## MODE 5: DASHBOARD — 대시보드 요구사항

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
