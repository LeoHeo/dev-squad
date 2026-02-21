---
name: test
description: Use when running backend tests - executes gradle test with optional test class filter
---

테스트를 실행해줘.

## 사전 준비

`.claude/project-config.json`을 읽어서 대상 디렉토리와 type을 확인해줘.
파일이 없으면 `/start-work`를 먼저 실행하라고 안내해줘.

## type별 테스트 명령어

| type | 기본 명령어 | 필터 명령어 |
|------|------------|------------|
| `spring-boot` (gradle) | `./gradlew test` | `./gradlew test --tests "{filter}"` |
| `spring-boot` (maven) | `./mvnw test` | `./mvnw test -Dtest="{filter}"` |
| `nuxt` / `nextjs` / `vite` / `node` | `npm run test:unit` | `npm run test:unit -- {filter}` |
| `go` | `go test ./...` | `go test ./... -run "{filter}"` |
| `rust` | `cargo test` | `cargo test {filter}` |
| 기타 | AskUserQuestion으로 테스트 명령어 확인 | - |

## 실행 규칙

1. project-config.json에서 대상 디렉토리 목록을 가져옴
2. 인자(`$ARGUMENTS`)가 있으면 해당 디렉토리의 type에 맞는 필터 명령어 사용
3. 인자가 없으면 기본 명령어 사용
4. 여러 디렉토리가 있으면 각각 순서대로 실행

## 결과 처리

- 성공: 테스트 결과 요약
- 실패: 실패 원인 분석 후 수정 코드 제시

인자: $ARGUMENTS
