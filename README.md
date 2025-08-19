# serverLessRestAPILambdaDynamoDB
Serverless REST API (Go or Node) on API Gateway + Lambda + DynamoDB  Pros: lean infra, cheap, great for OIDC-to-AWS in GitHub Actions, fast to iterate.  Extras: S3 + CloudFront for a tiny React UI, EventBridge for async jobs.

Repo layout

/app          # service code
/iac          # Terraform (environments: dev/stage/prod)
/.github
  /workflows  # ci.yml, cd.yml, security.yml


AWS auth: GitHub OIDC → AssumeRoleWithWebIdentity (least-privileged, per-env).

Quality gates (CI)

Lint: language linters (e.g., golangci-lint / eslint) + hadolint (Dockerfiles).

Security (SAST/IaC/Secrets/Deps/Container):

gitleaks (secrets)

trivy (deps + container + IaC) or checkov for Terraform

semgrep (SAST) (optional)

Tests: unit + integration (matrix by language/runtime).

SBOM: syft/cyclonedx (attach as artifact).

Build & package

Serverless: SAM/Serverless Framework or plain Terraform + zip.

Containers: Docker buildx → push to ECR (with provenance).

Deploy (CD)

Environments: dev → stage → prod, with required reviewers on prod.

Plan/apply split: terraform fmt/validate, tflint, terraform plan on PR; apply on merge.

App deploy: Lambda updates or ECS service rolling update (health checks).

Policies & protections

Branch protection, required checks, CODEOWNERS, conventional commits (optional).
