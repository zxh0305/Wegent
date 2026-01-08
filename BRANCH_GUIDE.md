# Development Branch Guide

## Branch Purpose

This guide documents the development workflow and branch strategy for the Wegent project.

### Branch Naming Convention

All development branches should follow the naming pattern: `weagent/<feature-or-task-name>`

**Examples:**
- `weagent/dev-initial-setup` - Initial development setup branch
- `weagent/feature-user-auth` - New authentication feature
- `weagent/fix-chat-bug` - Bug fix for chat functionality
- `weagent/refactor-backend-service` - Code refactoring work

### Active Development Branches

| Branch | Purpose | Status |
|--------|---------|--------|
| `weagent/dev-initial-setup` | Initial development branch for new features and code changes | Active |

## Development Workflow

### 1. Creating a New Development Branch

```bash
# Fetch latest remote branches
git fetch origin

# Create a new development branch from main
git checkout -b weagent/<feature-name>

# Push to remote
git push -u origin weagent/<feature-name>
```

### 2. Making Changes

Follow the Conventional Commits format for commit messages:

```
<type>[scope]: <description>
```

**Valid types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation
- `style` - Code style changes
- `refactor` - Code refactoring
- `test` - Test changes
- `chore` - Build, dependency, tool changes

**Example commits:**
```bash
git commit -m "feat(backend): add Ghost YAML import API"
git commit -m "fix(frontend): resolve chat message display bug"
git commit -m "refactor(executor): simplify task execution logic"
```

### 3. Code Quality Requirements

Before pushing changes:

1. **Run tests** (target: 40-60% coverage minimum):
   ```bash
   # Backend
   cd backend && uv run pytest --cov=app

   # Executor
   cd executor && uv run pytest

   # Executor Manager
   cd executor_manager && uv run pytest

   # Shared
   cd shared && uv run pytest

   # Frontend
   cd frontend && npm test

   # E2E Tests
   cd frontend && npm run test:e2e
   ```

2. **Format code**:
   ```bash
   # Backend
   cd backend && black . && isort .

   # Frontend
   cd frontend && npm run format
   ```

3. **Lint code**:
   ```bash
   # Frontend
   cd frontend && npm run lint
   ```

### 4. Creating a Merge Request

Once your changes are ready:

```bash
git push -u origin weagent/<feature-name>

# Use glab CLI to create MR
glab mr create \
  --title "Your MR Title" \
  --description "Detailed description of changes" \
  --target-branch main
```

### 5. Code Review and Merge

- Ensure all CI checks pass
- Address code review feedback
- Merge to `main` branch after approval
- Delete the feature branch after merging

## Project Structure

Key directories and their purposes:

```
wegent/
├── backend/              # FastAPI backend service
│   ├── app/
│   │   ├── api/          # Route handlers
│   │   ├── models/       # SQLAlchemy models
│   │   ├── schemas/      # Pydantic schemas
│   │   └── services/     # Business logic
│   └── alembic/          # Database migrations
├── frontend/             # Next.js frontend
│   └── src/
│       ├── components/   # React components
│       ├── features/     # Feature modules
│       └── hooks/        # Custom React hooks
├── executor/             # Task execution engine
├── executor_manager/     # Task orchestration
├── shared/               # Shared utilities
└── docker/               # Docker configurations
```

## Important Notes

- ⚠️ **All code comments must be written in English**
- ⚠️ **Always run tests before committing**
- ⚠️ **Do NOT use `git commit --no-verify` to skip hooks**
- ⚠️ **Python modules use `uv` for dependency management**
- ⚠️ **E2E tests must NOT fail gracefully - fix issues instead of skipping**

## Resources

- **Main Documentation**: See `/docs/en/` and `/docs/zh/` for comprehensive guides
- **CRD Architecture**: Refer to AGENTS.md for Kubernetes-inspired CRD design
- **Code Style**: See AGENTS.md for Python, TypeScript, and React standards

## Environment Setup

### Prerequisites

- Python 3.10+
- Node.js 18+
- Docker & Docker Compose
- Git & Git CLI

### Initial Setup

```bash
# Backend dependencies
cd backend && uv sync

# Frontend dependencies
cd frontend && npm install

# Executor dependencies
cd executor && uv sync

# Executor Manager dependencies
cd executor_manager && uv sync

# Shared dependencies
cd shared && uv sync
```

### Start Services

```bash
docker-compose up -d
```

**Service Ports:**
- Frontend: http://localhost:3000
- Backend API: http://localhost:8000
- Executor Manager: http://localhost:8001
- MySQL: localhost:3306
- Redis: localhost:6379

## Troubleshooting

### Git Issues

**Branch already exists locally:**
```bash
git branch -D weagent/<branch-name>
```

**Sync with latest main:**
```bash
git fetch origin
git rebase origin/main
```

**Undo last commit (keep changes):**
```bash
git reset --soft HEAD~1
```

### Python Issues

**Poetry/UV dependency conflicts:**
```bash
cd <module>
uv sync --refresh
```

### Frontend Issues

**Node modules issues:**
```bash
cd frontend
rm -rf node_modules package-lock.json
npm install
```

## Contributing Guidelines

1. Create descriptive branch names
2. Write clear, focused commit messages
3. Keep commits atomic and logical
4. Add tests for new features
5. Update documentation as needed
6. Follow project code style guidelines
7. Request review before merging

---

**Last Updated**: 2025-01-08
**Wegent Development Guide v1.0**
