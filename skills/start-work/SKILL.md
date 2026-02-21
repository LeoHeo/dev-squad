---
name: start-work
description: Use when starting a new task and need to prepare the working environment - project detection, uncommitted changes handling, base branch selection, and feature branch creation
---

새로운 작업을 시작하기 전 사전 준비를 수행해줘.
아래 단계를 순서대로 진행해줘.

## 대상 디렉토리 확인

`.claude/project-config.json` 파일을 읽어서 대상 디렉토리를 확인해줘.

파일이 없으면:
1. CWD 하위 디렉토리를 스캔하여 프로젝트 디렉토리를 자동 감지
2. 감지 기준:

| 파일 | 추정 type |
|------|-----------|
| `build.gradle` + Spring 의존성 | `spring-boot` |
| `pom.xml` + Spring 의존성 | `spring-boot` |
| `nuxt.config.ts` 또는 `nuxt.config.js` | `nuxt` |
| `next.config.*` | `nextjs` |
| `vite.config.*` | `vite` |
| `Cargo.toml` | `rust` |
| `go.mod` | `go` |
| `package.json`만 있음 | `node` |
| 기타 | `unknown` |

3. AskUserQuestion으로 각 디렉토리의 type 확인 (감지된 type을 추천, "직접 입력" 옵션 제공)
4. `.claude/project-config.json`을 생성:

```json
{
  "directories": [
    { "name": "backend", "path": "backend", "type": "spring-boot" },
    { "name": "frontend", "path": "frontend", "type": "nuxt" }
  ],
  "baseBranch": "master",
  "branchPrefix": "feature/"
}
```

---

## Step 1: 미커밋 변경사항 확인

대상 디렉토리들에서 각각 `git status --short`를 실행해줘.

변경사항이 있는 경우:
- 변경된 파일 목록을 보여주고
- AskUserQuestion으로 각 디렉토리별 처리 방법을 물어봐:
  - "stash로 저장" → `git stash push -m "auto-stash: $(date +%Y%m%d-%H%M%S)"` 실행
  - "무시하고 진행" → 그대로 진행
  - "작업 중단" → 여기서 멈춤

변경사항이 없으면 "깨끗합니다" 표시 후 다음 단계로.

## Step 2: 기본 브랜치 선택 + checkout

AskUserQuestion으로 새 브랜치의 기준이 될 base 브랜치를 물어봐:
- "master (기본)"
- "현재 브랜치 유지" → 현재 브랜치명을 표시하고 그대로 사용
- "직접 입력" → 사용자가 브랜치명 입력

선택 즉시 base 브랜치로 전환:
1. `git checkout {base-branch}`
2. `git pull origin {base-branch}` (실패해도 진행)

## Step 3: 새 브랜치 생성

사용자에게 이번 작업 내용을 간단히 물어본 뒤, 그 내용을 기반으로 브랜치명을 추천해줘.

AskUserQuestion으로 브랜치명을 결정:
- 추천 브랜치명 ({branchPrefix}작업내용-요약 형태, config의 branchPrefix 사용)
- "직접 입력"

결정된 후:
1. 새 브랜치 생성: `git checkout -b {new-branch}`

## Step 4: 결과 요약

아래 형식으로 결과를 보여줘:

```
작업 준비 완료!
─────────────────────────────
Base: {base-branch}
Branch: {new-branch}
─────────────────────────────
대상 디렉토리:
  {name}: {branch} ready ({type})
  ...
─────────────────────────────
Stash: {stash 했으면 내역, 없으면 "없음"}
─────────────────────────────
```
