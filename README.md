# Kubernetes Application Template Repository

This repository serves as a template for deploying applications to Kubernetes clusters using ArgoCD's "App of Apps" pattern. It provides a structured, maintainable approach to managing Kubernetes applications with built-in support for secrets management via SOPS, automated dependency updates with Renovate, and streamlined workflows through Taskfile.

## Components Overview

- **app1**: description
- **app2**: description

## Repository Structure

```
├── .taskfiles/                 # Custom Taskfile scripts modules
├── apps/                       # Core application configurations
├── argocd/                     # ArgoCD definitions
│   ├── app-of-apps.yaml
│   └── applications/
├── .envrc                      # Direnv environment variables
├── .gitattributes              # Git attributes configuration
├── .gitignore                  # Git ignore rules
├── .pre-commit-config.yaml     # Pre-commit hooks configuration
├── .sops.yaml                  # SOPS encryption configuration
├── gitleaks.toml               # Gitleaks secret scanning configuration
├── renovate.json               # Renovate dependency management
├── taskfile.yaml               # Task runner definitions
└── yamlfmt.yaml                # YAML formatting configuration
```

## Prerequisites

- [Task](https://taskfile.dev/) - Task runner tool
- [direnv](https://direnv.net/) - Environment variable management
- [SOPS](https://github.com/mozilla/sops) - Secret management
- [age](https://github.com/FiloSottile/age) - Encryption tool
- [uv](https://docs.astral.sh/uv/) - Python package management

## Usage Instructions

### 1. Initial Setup

Clone this repository and run the setup task to prepare your development environment:

```bash
git clone <your-repository-url>
cd <repository-name>
task setup
```

This will:
- Configure direnv for environment variables
- Install pre-commit hooks for code quality
- Set up SOPS differ for secure Git diff operations

### 2. SOPS Secret Management Setup

Generate an Age key for encrypting secrets:

```bash
# If $SOPS_AGE_KEY_FILE is exported trough direnv, set it first
age-keygen -o $SOPS_AGE_KEY_FILE
```

Copy the public key from the output and add it to `.sops.yaml` under the `age:` receivers:

```yaml
age:
  - age1haxzj5xhxtpag457wwykd4alk7x0cfdzc0gcfgltlvuh7zfywveqz53gjl # secrets-operator-key
  - <YOUR-AGE-PUBLIC-KEY-HERE> # Replace this line with your public key
```

### 3. Configure ArgoCD Integration

Update the repository URL in `argocd/app-of-apps.yaml`:

```yaml
spec:
  source:
    repoURL: <REPO-URL-HERE> # Replace with your Git repository URL
```

### 4. Add Your Applications

1. Add your application manifests to the `apps/` directory
1. Create ArgoCD application definitions in `argocd/applications/`
