# claude-project-switcher

Quick switch between projects with context loading.

## Installation

```bash
cd ~/.claude/plugins
git clone https://github.com/yangchoi/claude-project-switcher.git
```

## Setup

Create project paths config:

```bash
cat > ~/.claude/project-paths.json << 'EOF'
{
  "my-frontend": "/path/to/my-frontend",
  "my-backend": "/path/to/my-backend",
  "my-dashboard": "/path/to/my-dashboard"
}
EOF
```

## Usage

### Switch to project
```
/switch my-backend
/switch my-dashboard
```

### List projects
```
/switch list
```

### Switch to worktree
```
/switch my-frontend-wt
```

## What it does

1. Changes to project directory
2. Shows current git branch and status
3. Displays CLAUDE.md context (if exists)
4. Shows recent commits

## Example

```
/switch my-backend

# Output:
# Switched to: my-backend
# Branch: feature/user-auth
# Modified: 2 files
#
# === Context ===
# NestJS + GraphQL backend...
```

## License

MIT
