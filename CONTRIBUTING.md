# Contributing Guide

## Commit Message Convention

This project uses [Conventional Commits](https://www.conventionalcommits.org/).

### Format

```
<type>(<scope>): <subject>
```

### Scopes

- `terraform`: Changes to terraform module
- `shared`: Changes to shared module
- (none): Repository-level changes

### Types

- `feat`: New feature → MINOR version bump
- `fix`: Bug fix → PATCH version bump
- `feat!` or `BREAKING CHANGE:`: Breaking change → MAJOR version bump
- `chore`: Maintenance (no version bump)
- `docs`: Documentation (no version bump)

### Examples

```bash
feat(terraform): add tf/security-scan target
fix(shared): correct color reset sequence
feat(terraform)!: rename TF_ENV to ENVIRONMENT
chore: update README
```

## Release Process

1. Make changes to modules
2. Commit with Conventional Commits
3. Push to main
4. release-please creates Release PR automatically
5. Review and merge Release PR
6. Release and tags created automatically
