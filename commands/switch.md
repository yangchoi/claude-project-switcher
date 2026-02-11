---
name: switch
description: 프로젝트 빠른 전환. "/switch <project>", "/switch list" 사용
---

# Project Switcher

프로젝트 간 빠른 전환과 컨텍스트 로딩을 지원합니다.

## 사용법

### 프로젝트 전환
```
/switch my-frontend
/switch my-backend
/switch my-dashboard
/switch airflow
```

### 프로젝트 목록
```
/switch list
```

### Worktree로 전환
```
/switch my-frontend-wt
```

## 구현

### Step 1: 프로젝트 맵 정의

설정 파일: `~/.claude/project-paths.json`

```json
{
  "my-frontend": "/path/to/projects/my-frontend",
  "my-backend": "/path/to/projects/my-backend",
  "my-dashboard": "/path/to/projects/my-dashboard",
  "airflow": "/path/to/projects/airflow-dags",
  "my-frontend-wt": "/path/to/projects/my-frontend-wt"
}
```

### Step 2: 프로젝트 전환

1. 프로젝트 경로 조회
2. 해당 디렉토리로 이동
3. CLAUDE.md 읽기 (있으면)
4. git 상태 요약

```bash
PROJECT=$1
CONFIG_FILE=~/.claude/project-paths.json

# 경로 조회
PATH=$(jq -r --arg p "$PROJECT" '.[$p] // empty' "$CONFIG_FILE")

if [ -z "$PATH" ]; then
  echo "Unknown project: $PROJECT"
  echo "Available: $(jq -r 'keys | join(", ")' "$CONFIG_FILE")"
  exit 1
fi

# 디렉토리 이동
cd "$PATH"

# CLAUDE.md 읽기
if [ -f "CLAUDE.md" ]; then
  echo "=== Project Context ==="
  head -30 CLAUDE.md
fi

# Git 상태
echo ""
echo "=== Git Status ==="
git branch --show-current
git status -s | head -10
```

### Step 3: 컨텍스트 출력

전환 후 다음 정보 표시:
- 현재 브랜치
- 변경된 파일 (간략)
- CLAUDE.md 요약 (있으면)
- 최근 커밋 1-2개

## 예시

```
/switch my-backend

# 출력:
# Switched to: my-backend
# Path: /path/to/projects/my-backend
#
# === Git Status ===
# Branch: feature/user-auth
# Modified: 2 files
#
# === Project Context ===
# NestJS + GraphQL backend API.
# ...
```

## 설정

프로젝트 경로 추가/수정:

```bash
# ~/.claude/project-paths.json 직접 수정
# 또는
/switch add <name> <path>
```

## 팁

- 자주 쓰는 프로젝트는 짧은 별칭으로 등록
- worktree 경로도 등록해두면 편리
- `/switch list`로 등록된 프로젝트 확인
