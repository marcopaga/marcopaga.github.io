# Homepage

This page can be edited in a DevContainer which is specified in [.devcontainer/devcontainer.json](./.devcontainer/devcontainer.json). The container provides a hugo environment out of the box.

## Edit & Preview

Starting the local instance: `hugo server -D`

Access via [http://localhost:1313]

## Automated Dependency Management

This repository uses Renovate bot for automated dependency updates with built-in testing and auto-merge capabilities.

### How It Works

1. **Renovate bot** runs weekly (Mondays before 6am CET) and checks for dependency updates
2. **Test workflow** (`.github/workflows/test.yml`) automatically validates all PRs:
   - **Build test**: Verifies Hugo builds successfully
   - **Content validation**: Checks critical pages exist and have valid HTML structure
3. **Auto-merge**: Patch and minor updates auto-merge after tests pass
4. **Manual review**: Major updates and Hugo version changes require manual approval

### What Gets Auto-Merged

- ✅ Patch updates (e.g., v1.2.3 → v1.2.4)
- ✅ Minor updates (e.g., v1.2.0 → v1.3.0)
- ✅ GitHub Actions updates
- ✅ Ubuntu runner updates
- ✅ Docker image patches

### What Requires Manual Review

- ⚠️ Major version updates (e.g., v2.0.0 → v3.0.0)
- ⚠️ Hugo version updates (breaking changes common)
- ⚠️ Theme submodule updates

### Configuration Files

- `renovate.json`: Renovate bot configuration with auto-merge rules
- `.github/workflows/test.yml`: Automated testing workflow
- `.github/workflows/.github-pages.yml`: Deployment workflow
