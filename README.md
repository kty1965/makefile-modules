# Makefile Modules

Reusable, independently versioned Makefile modules managed with [release-please](https://github.com/googleapis/release-please).

## 🎯 Philosophy

Each module is independently versioned, allowing you to:
- Update only the modules you need
- Pin different versions for different modules
- Reduce breaking change impact

## 📦 Available Modules

| Module | Version | Description |
|--------|---------|-------------|
| **terraform** | v1.0.0 | Terraform workflow targets (init, plan, apply, etc.) |
| **shared** | v1.0.0 | Common utilities and helper functions |

Each module is independently versioned using [release-please](https://github.com/googleapis/release-please).

## 🚀 Quick Start

### Auto-download Pattern (Recommended)

Add to your `Makefile`:

```makefile
# 모듈별 버전 지정
TERRAFORM_MODULE_VERSION := v1.0.0
SHARED_MODULE_VERSION := v1.0.0

# 모듈별 태그 및 URL
TERRAFORM_TAG := modules-terraform-$(TERRAFORM_MODULE_VERSION)
SHARED_TAG := modules-shared-$(SHARED_MODULE_VERSION)

TERRAFORM_URL := https://github.com/kty1965/makefile-modules/releases/download/$(TERRAFORM_TAG)
SHARED_URL := https://github.com/kty1965/makefile-modules/releases/download/$(SHARED_TAG)

# 다운로드 규칙
make/terraform.mk:
	@mkdir -p make
	@curl -fsSL $(TERRAFORM_URL)/terraform.mk -o $@
	@curl -fsSL $(TERRAFORM_URL)/checksums.txt -o make/terraform-checksums.txt
	@cd make && grep "terraform.mk" terraform-checksums.txt | sha256sum -c -

make/shared.mk:
	@mkdir -p make
	@curl -fsSL $(SHARED_URL)/shared.mk -o $@
	@curl -fsSL $(SHARED_URL)/checksums.txt -o make/shared-checksums.txt
	@cd make && grep "shared.mk" shared-checksums.txt | sha256sum -c -

include make/shared.mk
include make/terraform.mk
```

Add to `.gitignore`:
```
make/*.mk
make/*-checksums.txt
```

### Vendoring Pattern (Offline Support)

```bash
# Download and commit to your repo
make vendor
git add vendor/make
git commit -m "chore: vendor terraform v1.0.0, shared v1.0.0"
```

In your `Makefile`:
```makefile
include vendor/make/shared.mk
include vendor/make/terraform.mk
```

## 📝 Usage Examples

See consumer demo at [kty1965/tf-consumer-demo](https://github.com/kty1965/tf-consumer-demo).

## 🔄 Updating Modules

### Update Single Module

```makefile
# Update only terraform module
TERRAFORM_MODULE_VERSION := v1.2.0
SHARED_MODULE_VERSION := v1.0.0  # Keep current version

# Clean and re-download
make modules/clean
make build
```

### Update All Modules

```bash
# Edit versions in Makefile
TERRAFORM_MODULE_VERSION := v1.2.0
SHARED_MODULE_VERSION := v1.1.0

make modules/update
```

## 📖 Module Documentation

### terraform.mk

Provides Terraform workflow targets:

- `tf/init`: Initialize Terraform
- `tf/plan`: Generate execution plan
- `tf/apply`: Apply changes
- `tf/validate`: Validate configuration
- `tf/fmt`: Format Terraform files
- `tf/state-list`: List state resources
- `tf/output`: Show outputs

See [modules/terraform/](modules/terraform/) for full documentation.

### shared.mk

Provides common utilities:

- `log_info`, `log_success`, `log_warn`, `log_error`: Colored logging
- `clean`: Clean build artifacts
- `version`: Show project version

See [modules/shared/](modules/shared/) for full documentation.

## 🔐 Security

All releases include `checksums.txt` for integrity verification:

```bash
cd make
sha256sum -c terraform-checksums.txt
sha256sum -c shared-checksums.txt
```

## 📋 Versioning

Each module is independently versioned using [Semantic Versioning](https://semver.org/) and [release-please](https://github.com/googleapis/release-please):

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

Conventional Commit prefixes:
- `feat(terraform):` - New feature for terraform module → MINOR bump
- `fix(shared):` - Bug fix for shared module → PATCH bump
- `feat(terraform)!:` or `BREAKING CHANGE:` → MAJOR bump

See each module's CHANGELOG.md for version history.

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## 📄 License

MIT
