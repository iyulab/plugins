---
description: Execute iterative development cycles - scope→research→implement→test→evaluate→improve loop
argument-hint: "[total_cycles] [start_cycle]" [--dry-run] [--no-commit]
disable-model-invocation: true
allowed-tools: Read, Glob, Grep, Write, Edit, TodoWrite, WebFetch, WebSearch, Bash
---

# Development Cycle Runner

자동화된 반복 개발 사이클을 실행한다.
**중단 없이** 모든 사이클을 순차적으로 완료한다.

## Philosophy

**"Iterative Excellence"** - 한 사이클에서 완벽할 필요는 없다. 매 사이클이 이전보다 나아지면 된다.

**"Research before code"** - 모르면 조사한다. 추측으로 구현하지 않는다.

**"Scope discipline"** - 한 사이클에 완료 가능한 크기만 잡는다. 욕심이 품질을 죽인다.

**"Honest evaluation"** - 자기 코드에 관대하지 않는다. 결함은 숨기지 않고 기록한다.

## Mindset: Critical but Constructive

**"Passionate Developer"** - 수동적 실행자가 아닌, 프로젝트 성공에 깊이 관여하는 개발자.

**"1 → 10 thinking"** - 하나의 구현에서 열 가지 함의를 도출한다.

**"Proactive research"** - 불확실하면 WebSearch를 적극 활용하여 모범 사례, 패턴, 함정을 조사한다.

**"Be your own QA"** - 테스트, 검증, 시뮬레이션. 실패를 기다리지 말고 예측한다.

**"Defend the architecture"** - 편의를 위한 아키텍처 위반을 거부한다.

## Parameters

- 총 사이클 수: `$ARGUMENTS[0]` (기본값: 5)
- 시작 사이클 번호: `$ARGUMENTS[1]` (기본값: 1)

**Flags**:
- `--dry-run`: 준비 + 로드맵 수립까지만 (실행 안 함)
- `--no-commit`: 실행하되 커밋 생략

---

## Phase 0: Preparation

사이클 실행 전 반드시 수행한다:

### 0.1 Project Context

```bash
# 프로젝트 원칙 파악
Read: CLAUDE.md (or .claude/CLAUDE.md)

# 프로젝트 구조 파악
LS: project root
Read: README.md, package.json / Cargo.toml / pyproject.toml / *.csproj (해당하는 것)
```

**파악 항목**:
- 프로젝트의 **핵심 역할** (무엇을 하는 라이브러리/앱인가?)
- 프로젝트의 **범위 경계** (무엇을 하지 않는가?)
- 아키텍처 원칙 및 코딩 규칙
- 기술 스택 및 의존성

### 0.2 Previous Cycle Logs

```bash
# 이전 사이클 기록 확인
Glob: logs/cycle-*.md
# 가장 최근 로그 읽기 (있다면)
```

- 이전 사이클에서 도출된 결함/개선점 확인
- "Next Cycle Recommendation" 항목 반드시 참조

### 0.3 Current State Assessment

```bash
# 최근 변경 사항 확인
git log --oneline -20
git diff HEAD~5 --stat

# 빌드/테스트 상태 확인
# (프로젝트 빌드 시스템에 맞게 실행)
```

### 0.4 Development Plan Discovery

**Search Order** (stop at first found):
1. CLAUDE.md - Development plan section
2. ROADMAP.md / roadmap.md
3. TASKS.md / TODO.md
4. docs/ - Planning documents
5. README.md - "Roadmap", "Planned Features" sections

```bash
Glob: **/ROADMAP.md, **/TASKS.md, **/TODO.md, docs/*.md
Grep: "## Roadmap|## TODO|## Planned|## Tasks|## Development Plan" in *.md
```

### 0.5 Roadmap Establishment

**로드맵이 존재하면**: 읽고 현재 진행 상태를 파악한다.

**로드맵이 없으면**: 프로젝트 상태를 분석하여 `logs/ROADMAP.md`에 작성한다.
- 총 사이클 수에 맞춘 단계적 계획
- 각 사이클의 목표와 범위 개요
- 사이클 간 의존성 표시

```
PREPARATION COMPLETE
+--------------------+--------------------------------------------+
| Project            | [name]                                     |
| Role               | [library / app / framework / tool]         |
| Tech Stack         | [language, key dependencies]               |
| Previous Cycles    | [N completed / fresh start]                |
| Roadmap            | [existing / newly created]                 |
| Total Cycles       | [N planned]                                |
| Starting From      | Cycle [M]                                  |
+--------------------+--------------------------------------------+
```

**If --dry-run, stop here.**

---

## Per-Cycle Process

**모든 사이클은 아래 5단계를 엄격히 따른다.**

---

### STEP 1: Scope Setting (구현 범위 설정)

로드맵과 이전 사이클 로그를 기반으로 이번 사이클의 범위를 결정한다.

#### 1.1 Scope Definition

- 한 사이클 내에서 **완료 가능한 크기**로 제한
- 구체적이고 검증 가능한 목표 설정
- 산출물(deliverables) 명시

#### 1.2 Philosophy Alignment

프로젝트 철학과의 정렬을 평가한다:

| Dimension | Score 1-5 | Assessment |
|-----------|-----------|------------|
| **Core Mission Fit** | | 프로젝트 핵심 역할에 기여하는가? |
| **Scope Boundaries** | | 프로젝트 범위 내에 있는가? |
| **Architecture Patterns** | | 기존 아키텍처 패턴과 일관되는가? |
| **Dependency Direction** | | 의존성 방향이 올바른가? (상위→하위 유입 없는가?) |

**Decision**:
```
             | Scope IN        | Scope OUT       |
-------------|-----------------|-----------------|
Mission HIGH | PROCEED         | ADAPT scope     |
Mission MED  | ADAPT approach  | REDUCE scope    |
Mission LOW  | RECONSIDER      | SKIP            |
```

#### 1.3 Role & Scope Check

라이브러리/프로젝트의 역할과 범위를 점검한다:
- 이 구현이 이 프로젝트의 **책임**인가?
- 다른 모듈/패키지에서 해야 할 일이 아닌가?
- 상위 소비자의 관심사가 유입되지 않는가?

```
CYCLE [N] SCOPE
+--------------------+--------------------------------------------+
| Title              | [사이클 제목]                               |
| Objective          | [구체적 목표]                               |
| Deliverables       | [산출물 목록]                               |
| Philosophy Score   | [avg / 5]                                  |
| Decision           | [PROCEED / ADAPT / REDUCE / SKIP]          |
+--------------------+--------------------------------------------+
```

---

### STEP 2: Research & Implementation (연구 & 구현)

#### 2.1 Research Phase (연구 선행)

**구현 전에 반드시 조사한다.**

Task 도구로 subagent를 활용하거나 직접 조사한다:

| Research Area | Tools | Purpose |
|---------------|-------|---------|
| Best Practices | WebSearch | 해당 기능의 업계 모범 사례 |
| Existing Patterns | Grep, Glob | 코드베이스 내 유사 패턴 |
| External References | WebSearch, WebFetch | 라이브러리 문서, API 레퍼런스 |
| Edge Cases | WebSearch | 알려진 엣지 케이스, 함정 |
| Security | WebSearch | 보안 고려 사항 (해당 시) |

**Research Triggers** (자동):
- 코드베이스에 없는 새로운 기술/패턴
- 외부 통합 필요
- 보안/성능 관련
- 복잡한 알고리즘/로직

**Output**:
```
RESEARCH SUMMARY
- [topic]: [findings] ([source])
- [topic]: [findings] ([source])
Key Decision: [연구 결과에 기반한 구현 방향]
```

#### 2.2 Implementation Phase (구현)

연구 결과를 바탕으로 코드를 작성한다:

- **프로젝트 코딩 규칙 준수** (CLAUDE.md 참조)
- **테스트 동반 작성**: 구현과 함께 테스트를 작성한다
- **점진적 진행**: 한 번에 전부가 아닌, 단위별로 구현 + 검증
- **Root Cause Mindset**: 문제 발견 시 근본 원인 해결

**진행 추적**:
```bash
TodoWrite: 각 하위 작업의 진행 상태 추적
```

---

### STEP 3: Test & Verify (테스트 & 검증)

프로젝트 기술 스택에 맞는 테스트를 실행한다:

| Tech Stack | Test Commands |
|------------|---------------|
| Node.js/TS | `npm test`, `npm run lint`, `npm run build` |
| Rust | `cargo test`, `cargo clippy -- -D warnings` |
| Python | `pytest`, `ruff check`, `mypy` |
| .NET | `dotnet test`, `dotnet build` |
| Go | `go test ./...`, `go vet ./...` |
| Generic | README.md 또는 CI 설정에서 테스트 명령어 파악 |

**검증 절차**:
1. 전체 테스트 스위트 실행
2. 린트/정적 분석 실행
3. 빌드 확인
4. **실패 시**: 수정하고 재실행 (자가 복구)
5. 테스트 커버리지가 부족하면 추가 테스트 작성

```
TEST RESULTS
+--------------------+--------------------------------------------+
| Tests              | [passed] / [total] ([failures] failed)     |
| Lint               | [PASS / N warnings / N errors]             |
| Build              | [SUCCESS / FAILURE]                        |
| Coverage           | [sufficient / needs improvement]           |
+--------------------+--------------------------------------------+
```

---

### STEP 4: Objective Evaluation (객관적 평가)

다음 기준으로 이번 사이클의 결과를 **1~10점**으로 평가한다:

| Criterion | Description | Score |
|-----------|-------------|-------|
| **Correctness** | 기능이 정확하게 동작하는가? 엣지 케이스는? | /10 |
| **Architecture** | 설계가 프로젝트 아키텍처와 일관되는가? 적절한 추상화인가? | /10 |
| **Philosophy Alignment** | 프로젝트 철학/원칙에 부합하는가? 역할 범위 내인가? | /10 |
| **Test Quality** | 의미 있는 테스트가 충분한가? 핵심 시나리오를 커버하는가? | /10 |
| **Documentation** | 코드가 자기 설명적인가? 필요한 문서가 있는가? | /10 |
| **Code Quality** | 언어 관용구 준수, 가독성, 유지보수성은 적절한가? | /10 |

**평가 원칙**:
- **7점 이하**: 구체적 결함과 개선 방향 기록 필수
- **자기 관대 금지**: 작동한다 ≠ 좋은 코드
- **상대적 평가 금지**: 프로젝트의 기준에 맞춰 절대 평가

```
EVALUATION
+------------------------+-------+----------------------------------+
| Criterion              | Score | Notes                            |
+------------------------+-------+----------------------------------+
| Correctness            | /10   |                                  |
| Architecture           | /10   |                                  |
| Philosophy Alignment   | /10   |                                  |
| Test Quality           | /10   |                                  |
| Documentation          | /10   |                                  |
| Code Quality           | /10   |                                  |
+------------------------+-------+----------------------------------+
| AVERAGE                | /10   |                                  |
+------------------------+-------+----------------------------------+
```

---

### STEP 5: Issues & Improvements (결함/미비/개선점 도출)

#### 5.1 Deficiency Analysis

평가에서 7점 미만인 항목의 **구체적 결함**을 기록한다:
- 무엇이 부족한가?
- 어떻게 개선할 수 있는가?
- 다음 사이클에서 해결 가능한가?

#### 5.2 Architectural Discoveries

사이클 진행 중 발견한 아키텍처 관련 사항:
- 설계 변경 필요성
- 기술 부채 식별
- 리팩토링 제안

#### 5.3 Next Cycle Recommendation

다음 사이클을 위한 구체적 권고:
- 우선 해결할 사항
- 범위 조정 필요성
- 선행 조건

```
ISSUES & IMPROVEMENTS
+------+----------+-----------------------------------------+--------+
| #    | Severity | Description                             | Action |
+------+----------+-----------------------------------------+--------+
| I-01 | [H/M/L]  | [issue description]                     | [next] |
| I-02 | [H/M/L]  | [issue description]                     | [next] |
+------+----------+-----------------------------------------+--------+
```

---

## Cycle Log Format

각 사이클 완료 시 `logs/cycle-{NN}.md` 파일을 생성한다.

```markdown
# Cycle {NN}: {Title}

## Date
{YYYY-MM-DD}

## Scope
{이번 사이클의 구현 범위}

## Philosophy Alignment
| Dimension | Score |
|-----------|-------|
| Core Mission Fit | /5 |
| Scope Boundaries | /5 |
| Architecture Patterns | /5 |
| Dependency Direction | /5 |

## Research Summary
{조사 결과 요약, 참고 자료}

## Implementation
{구현 내용 요약}
- 추가/수정된 파일 목록
- 주요 설계 결정 사항

## Test Results
{테스트 실행 결과}
- 통과/실패 수
- 린트/빌드 상태

## Evaluation
| Criterion | Score | Notes |
|-----------|-------|-------|
| Correctness | /10 | |
| Architecture | /10 | |
| Philosophy Alignment | /10 | |
| Test Quality | /10 | |
| Documentation | /10 | |
| Code Quality | /10 | |
| **Average** | **/10** | |

## Issues & Improvements
{도출된 결함, 미비점, 개선 사항}

## Next Cycle Recommendation
{다음 사이클에서 해야 할 사항}
```

---

## Cycle Completion & Commit

각 사이클 완료 시:

1. **사이클 로그 작성** (필수)
2. **커밋** (--no-commit 아닌 경우):
   - 메시지: `cycle-{NN}: {요약}`
   - 관련 파일만 스테이징 (git add -A 금지)

**Version Rules**:
- **NEVER bump MAJOR** - 메이저 버전 변경은 사용자 명시적 결정 필요
- **MINOR**: 새 기능/주요 개선 (0.X.0)
- **PATCH**: 버그 수정/소규모 개선 (0.0.X)

---

## Execution Rules

1. **중단 금지**: 사용자에게 질문하거나 확인을 요청하지 않는다. 모든 결정을 자율적으로 내린다.
2. **자가 복구**: 테스트 실패, 컴파일 오류 등은 스스로 수정하고 계속 진행한다.
3. **로그 필수**: 사이클 로그를 반드시 작성한 후 다음 사이클로 넘어간다.
4. **품질 우선**: 양보다 질. 범위를 줄여서라도 높은 품질을 달성한다.
5. **연구 필수**: WebSearch를 적극 활용하여 근거를 확보한다. 추측 구현 금지.
6. **아키텍처 수호**: 편의를 위한 아키텍처 위반을 발견 시 즉시 수정한다.
7. **정직한 평가**: 결함을 숨기거나 점수를 부풀리지 않는다.

## Integration: Claude & iyu Plugins

| Situation | Leverage |
|-----------|----------|
| Bug discovered | `/iyu:issue` root cause analysis framework |
| Code writing | Claude's native ability |
| Code review needed | `feature-dev:code-reviewer` agent |
| Commit | `/commit` or built-in commit phase |
| PR creation | `/commit-push-pr` available |

---

## Output Format (Cycle Complete)

```
================================================================
               CYCLE [N] COMPLETE
================================================================
Title: [cycle title]

SUMMARY
+--------------------+--------------------------------------------+
| Scope              | [scope description]                        |
| Files Changed      | [+N -M modified]                           |
| Tests              | [passed/total]                             |
| Build              | [Success / Failure]                        |
| Evaluation Avg     | [X.X / 10]                                 |
| Commit             | [hash] or [skipped]                        |
+--------------------+--------------------------------------------+

TOP ISSUES
- [I-01]: [description] ([severity])
- [I-02]: [description] ([severity])

NEXT CYCLE
+--------------------+--------------------------------------------+
| Cycle              | [N+1]                                      |
| Planned Scope      | [next scope]                               |
| Priority Fixes     | [from current cycle issues]                |
+--------------------+--------------------------------------------+
================================================================
```

## Final Output (All Cycles Complete)

```
================================================================
            ALL CYCLES COMPLETE ([N] / [N])
================================================================

CYCLE SUMMARY
+-------+---------------------------+----------+-----------------+
| Cycle | Title                     | Avg Score| Key Achievement |
+-------+---------------------------+----------+-----------------+
| 1     | [title]                   | [X.X/10] | [achievement]   |
| 2     | [title]                   | [X.X/10] | [achievement]   |
| ...   | ...                       | ...      | ...             |
+-------+---------------------------+----------+-----------------+

OVERALL METRICS
+--------------------+--------------------------------------------+
| Total Files        | [+N -M modified]                           |
| Total Commits      | [N]                                        |
| Average Score      | [X.X / 10]                                 |
| Trend              | [improving / stable / declining]            |
+--------------------+--------------------------------------------+

OUTSTANDING ISSUES
- [unresolved issues across all cycles]

RECOMMENDATIONS
- [strategic recommendations for future development]
================================================================
```

## Execution Start

위 규칙에 따라 지금 즉시 실행을 시작한다.
Phase 0: Preparation → Roadmap → Cycle 1 → Cycle 2 → ... → Cycle N 까지 완전 자동으로 실행한다.
