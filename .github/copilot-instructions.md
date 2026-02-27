<!-- .github/copilot-instructions.md - guidance for AI coding agents working in this repo -->

# Quick instructions for AI agents

This repository is a small Azure DevOps lab scaffold. The guidance below is intentionally focused and actionable so an AI can be immediately productive.

1) Big picture (what this repo is for)
   - Layout: top-level folders are `bicep/`, `terraform/`, `pipelines/`, and `docs/` with a brief root `README.md`.
   - Purpose: the project is an infrastructure-as-code lab for Azure. Expect Bicep and/or Terraform to declare resources, and Azure DevOps pipeline YAML under `pipelines/` to orchestrate CI/CD.

2) Where to look first
   - Root README: `README.md` (currently minimal) — use it for high-level notes.
   - IaC: check `bicep/` for `.bicep` files and `terraform/` for `.tf` files.
   - Pipelines: `pipelines/` should contain Azure DevOps YAML pipelines (`azure-pipelines.yml` or similar).
   - Docs: `docs/` is for runbooks, design notes, and commands used by humans.

3) Concrete, discoverable patterns and commands
   - If you find `.bicep` files: prefer Bicep tooling. Common commands (run locally or in CI):
     - `bicep build <file>.bicep` to validate & compile to ARM JSON
     - `az deployment group create --resource-group <rg> --template-file <file>.json` to test deployment (requires Azure credentials)
   - If you find Terraform files: use standard Terraform lifecycle commands:
     - `terraform init` then `terraform plan -out=plan` then `terraform apply plan`
   - Pipelines: look for `azure-pipelines.yml` or files under `pipelines/`. Azure DevOps pipelines expect the YAML in the pipeline configuration; verify triggers and variable groups there.

4) Integration points and assumptions
   - Azure: the repo targets Azure. Any deployment or pipeline work assumes Azure CLI (`az`) and appropriate service principal credentials or Azure DevOps variable groups.
   - CI: pipelines likely reference service connections/variable groups not present in the repo. Do not hardcode secrets—use placeholders and document required variable names in `docs/`.

5) Project-specific conventions (inferred from structure)
   - Keep IaC under `bicep/` or `terraform/` depending on which language is used for a given module. Don’t mix resource definitions for the same logical module across both languages.
   - Place pipeline YAML under `pipelines/` and name them descriptively (e.g., `ci.yml`, `deploy.yml`, or `azure-pipelines.yml`).
   - Use `docs/` for operational notes: commands to run locally, required Azure resource group names, and expected variable names for pipelines.

6) What to do when files are missing or the repo is empty
   - This repo currently contains only `README.md` and empty folders. If asked to implement a feature:
     - Create minimal, well-documented scaffolding in the appropriate folder (example: add `bicep/main.bicep` with a simple storage account, or `pipelines/ci.yml` that runs `bicep build`/`terraform validate`).
     - Add a short `docs/README.md` describing how to run the scaffold locally and what secrets/variables are required.
     - Prefer small, focused changes and include a README note clarifying assumptions (subscription, resource group names, credentials).

7) PR and commit guidance for AI agents
   - Keep commits focused (one logical change per PR). Include a short PR description with:
     - What was added/changed
     - Commands to validate locally
     - Any required pipeline or Azure DevOps setup (service connections, variable group names)

8) Safety and secrets
   - Never create or expose real secrets (service principal creds, client secrets, connection strings) in the repo. Use placeholder names like `<AZURE_SP_SECRET>` and document how to set them in the pipeline or `docs/`.

9) When in doubt
   - Ask a human reviewer for missing operational details (subscription id, default resource group, variable group names). If a human is not available, create clear assumptions in `docs/` and in the PR description.

If any part of this guidance is unclear or you'd like more automation (scaffolding, example pipelines, or a local test harness), tell me which area to expand and I will iterate.
