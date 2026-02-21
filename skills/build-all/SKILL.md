---
name: build-all
description: Use when building both backend and frontend projects sequentially - runs gradle build then npm build and reports results
---

모든 대상 디렉토리를 순서대로 빌드해줘.

## 사전 준비

`.claude/project-config.json`을 읽어서 대상 디렉토리와 type을 확인해줘.
파일이 없으면 `/start-work`를 먼저 실행하라고 안내해줘.

## type별 빌드 명령어

| type | 빌드 명령어 |
|------|------------|
| `spring-boot` (gradle) | `./gradlew clean build` |
| `spring-boot` (maven) | `./mvnw clean package` |
| `nuxt` / `nextjs` / `vite` / `node` | `npm run build` |
| `go` | `go build ./...` |
| `rust` | `cargo build --release` |
| 기타 | AskUserQuestion으로 빌드 명령어 확인 |

## 실행 규칙

1. project-config.json의 directories 배열 순서대로 빌드
2. 각 디렉토리의 type에 맞는 빌드 명령어 실행
3. 각 단계의 성공/실패 결과를 요약

실패 시 원인을 분석하고 수정 방안을 제시해줘.
