# Description
<!-- Provide a short description of the changes in this PR -->
## Details
<!-- Add implementation details that help reviewers understand the approach. -->

## Linked Work Item
<!-- Link the ServiceNow story or ticket. -->
STRYXXXXXXX

## Higher Environment Deployment Changes commands
<!-- any dbt commands, or sql commands 
```bash 
 dbt build --full-refresh --select <model name>
```
-->

## Risk and Rollback
<!-- Describe deployment risk and rollback plan. -->

## Testing
- [ ] Local testing performed
- [ ] DAG validation passed (if applicable)
- [ ] Unit/integration tests added or updated (if applicable)
- [ ] Test evidence added below

Test Evidence:
<!-- Add logs, screenshots, command output, or links to CI runs. -->

# Checks

I have:

- [ ] Performed a self-review of my code
- [ ] New and existing unit tests pass locally
- [ ] Added test evidence to the PR description
- [ ] Code is readable and respects agreed linting standards 
- [ ] Added relevant documentation
- [ ] Commented my code, especially in hard-to-understand areas

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update
- [ ] Refactoring / code cleanup

<!--  Semantic PR Use semantic PR format:

`<type>(<scope>): <short summary>`

Examples:
- `feat(release): add tag validation for manual promotions`
- `fix(ci): handle empty branch names in merge step`
- `docs(readme): clarify release command usage`

Allowed types:
- `feat`: New functionality
- `fix`: Bug fix
- `docs`: Documentation-only change
- `refactor`: Internal change without behavior change
- `test`: Add or update tests
- `chore`: Maintenance, tooling, or dependency updates
- `ci`: Pipeline or automation updates
- `perf`: Performance improvement
- `revert`: Revert a previous change
-->