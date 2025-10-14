# GitHub Copilot Agent Onboarding

## 1. Repository Overview
- **Purpose**: A composite GitHub Action to automate semantic version bumps, changelog generation, GitHub Release creation, and multi-language package publication (npm, PyPI, NuGet, Go, Maven) for AWS CDK constructs.
- **Key Files**  
  - `action.yml`: inputs and step definitions.  
  - `package.json`: scripts (`ci:gha`, `jsii:pacmak:parallel`, `publish:*`).  
  - `.github/workflows/`: CI pipelines (build, release, PR lint, labeler, queue, auto-approve).  
  - `.vscode/settings.json`: formatting and editor preferences.  

## 2. Local Environment Setup (Always start from a clean clone)
1. **Install Node.js v24.9.0 & Yarn**  
   ```bash
   curl -fsSL https://nodejs.org/dist/v24.9.0/node-v24.9.0-linux-x64.tar.xz -o node.tar.xz
   sudo tar -xJf node.tar.xz -C /usr/local
   export PATH="/usr/local/node-v24.9.0-linux-x64/bin:$PATH"
   corepack enable && corepack prepare yarn@latest --activate
   ```
2. **Install dependencies**  
   ```bash
   yarn install
   ```

## 3. Build & Validation
- **Lint & Test**  
  ```bash
  yarn run ci:gha
  ```  
  (runs all lint, compile, and test steps; verify exit code is zero)
- **Package**  
  ```bash
  yarn run jsii:pacmak:parallel
  ```
- **Publish Dry-Run**  
  Set environment variables (`NPM_TOKEN`, `TWINE_USERNAME`, `TWINE_PASSWORD`, `NUGET_API_KEY`, `GO_GITHUB_TOKEN`) and run:  
  ```bash
  yarn run publish:npm
  yarn run publish:pypi
  yarn run publish:nuget
  yarn run publish:golang
  ```

## 4. CI Workflows
- **build.yml**: no-op build to verify repo integrity.
- **release.yml**: on `main` push, bumps version, creates changelog, and publishes a GitHub Release.
- **PR lint**: enforces semantic‐commit style.
- **PR labeler**, **auto-queue**, **auto-approve**: manage PR metadata, merge queues, and approvals automatically.

## 5. Project Layout & Validation Steps
- Root:
  - `action.yml`, `package.json`, `README.md`, `LICENSE`
- Directories:
  - `.github/workflows/` (YAML CI pipelines)
  - `.vscode/` (editor settings)
  - `dist/` (generated artifacts)
- **Pre-submission checklist for any PR**:
  1. `yarn install`
  2. `yarn run ci:gha`
  3. `yarn run jsii:pacmak:parallel`
  4. Verify any changed workflow with a `yamllint` or equivalent.
- **Agent note**: Rely on these documented steps. Only search the repo when you encounter missing or outdated information.
