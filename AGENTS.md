---
name: Developer
description: DevOps cicd — owasp
---

## Tech Stack
- **Language:** Bash / Shell
- **Deployment:** Shell scripts
- **CI/CD:** GitHub Actions (.github/workflows)

## Commands
```bash
deploy.sh <ENVIRONMENT> <VERSION>
```

## Project Structure
```
scripts/
deploy.sh
sonar-project.properties
README.md
.github/workflows/
```

## Code Standards
**Naming conventions:**
- Repositories: kebab-case (`devops-{category}-{name}`)
- Files: kebab-case (`deploy.sh`, `template.yml`)
- Variables: snake_case for shell scripts


**Patterns — as used in this repo:**

```bash
# ✅ Good — well-documented shell function with exit code check
deploy_app() {
    local env=$1 version=$2
    echo "Deploying version ${version} to ${env}..."
    aws cloudformation deploy --template-file template.yml --parameter-overrides Environment="${env}" Version="${version}"
}

# ❌ Avoid — no error handling, no parameter validation
function bad_deploy() {
    aws cloudformation deploy --template-file template.yml
}
```

## Boundaries
### ✅ Always
- Run build && deploy before commits
- Follow the CI/CD pipeline in .github/workflows
- Use vars/ for environment-specific configuration
- Follow project naming conventions
- Run syntax checks before committing
- Handle pagination for AWS API calls
### ⚠️ Ask first
- Adding new infrastructure resources
- Changing CI/CD configuration (.github/workflows, deploy.yml)
- Modifying deployment accounts or roles
- Adding new environments
- Large refactors affecting multiple services
### 🚫 Never
- Commit secrets, API keys, or hardcoded credentials
- Modify .git/ or version control internals
- Remove or disable monitoring/alerting without approval
- Skip pre-deployment checks
