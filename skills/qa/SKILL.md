---
name: qa
description: Use when running QA tests - Intent-based UI automation testing with Playwright, auto-healing for failed steps, and UX/UI improvement analysis
---

시니어 QA Agent로 Intent 기반 UI 자동화 테스트를 수행해줘.

## 대상 디렉토리 확인

`.claude/project-config.json` 파일을 읽어서 대상 디렉토리와 type을 확인해줘.
파일이 없으면 `/new-feature` 또는 `/start-work` 스킬을 먼저 실행하라고 안내해줘.

---

## 사전 확인 (자동 서버 시작)

project-config.json의 각 디렉토리 type에 따라 서버를 확인하고, 미실행 시 자동으로 시작해줘.

### type별 서버 시작 명령어

| type | 실행 명령어 | 기본 포트 | health check URL |
|------|------------|----------|-----------------|
| `spring-boot` | `./gradlew bootRun` | 8080 | http://localhost:8080 |
| `nuxt` | `npm run dev` | 3000 | http://localhost:3000 |
| `nextjs` | `npm run dev` | 3000 | http://localhost:3000 |
| `vite` | `npm run dev` | 5173 | http://localhost:5173 |
| `go` | `go run .` | 8080 | http://localhost:8080 |
| `rust` | `cargo run` | 8080 | http://localhost:8080 |
| `node` | `npm start` | 3000 | http://localhost:3000 |
| 기타 | AskUserQuestion으로 실행 명령어와 포트 확인 | - | - |

### 서버 시작 절차 (각 디렉토리별)

1. health check URL로 응답 확인:
   `curl -s -o /dev/null -w "%{http_code}" {url} --connect-timeout 3`
2. 응답 없으면 → 해당 디렉토리에서 실행 명령어를 **백그라운드**로 실행
3. 최대 30초 대기하면서 health check 반복 (2초 간격)
4. 30초 초과 시 사용자에게 안내하고 중단

모든 서버가 응답하면 QA 진행.

---

## 실행 모드 선택

AskUserQuestion으로 실행 모드를 물어봐:

- "전체 테스트" → `docs/qa/scenarios/` 의 모든 시나리오 실행
- "특정 페이지" → 페이지 목록 표시 후 선택
- "개선점 도출" → QA Agent 실행 후 UX/UI Reviewer Agent로 개선점 분석
- "재검증" → 이전 결과에서 실패한 항목만 재실행

---

## 실행

### 전체 테스트 / 특정 페이지

1. QA Agent(`.claude/agents/qa-agent.md`)를 Task 도구로 실행
2. 시나리오 파일을 순서대로 처리
3. 각 Step의 Intent를 Playwright로 실행
4. Expected vs Actual 비교 후 결과 문서 생성
5. 결과를 `docs/qa/results/YYYY-MM-DD/` 에 저장
6. 실패 항목이 있으면 Auto-Healing 시도 (qa-agent.md의 Auto-Healing 절차에 따라 3단계 실행)
7. Healing 결과를 `docs/qa/healing-log/YYYY-MM-DD.healing.md` 에 자동 기록

### 개선점 도출

1. 먼저 전체 테스트 실행 (위와 동일)
2. 결과 문서를 UX/UI Reviewer Agent(`.claude/agents/uxui-reviewer.md`)에 전달
3. 접근성, 사용성, 일관성 관점에서 개선점 분석
4. 개선점을 `docs/qa/improvements/` 에 저장

### 재검증

1. 가장 최근 결과 폴더에서 `fail` 상태인 Step 추출
2. 해당 시나리오만 재실행
3. 이전 결과와 비교하여 개선 여부 표시
4. 재검증 완료 후 Security Senior Agent로 보안 관점 검증 수행 (프로젝트에 별도 security-auditor가 있으면 우선 사용)
5. 코드 품질 관점 검증 수행

---

## 결과 요약

실행 완료 후 아래 형식으로 요약해줘:

```
QA 테스트 결과 요약
─────────────────────────────
실행일: YYYY-MM-DD
모드: {전체/특정/개선점/재검증}
─────────────────────────────
PASS: N 시나리오
FAIL: N 시나리오
PARTIAL: N 시나리오
─────────────────────────────
총 Steps: N / N passed
Auto-Healing: N건 적용
─────────────────────────────
결과: docs/qa/results/YYYY-MM-DD/
```
