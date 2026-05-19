# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x     | ✅ Active development |

## Reporting a Vulnerability

If you discover a security vulnerability in the Cloud Cost Optimization & FinOps Framework, please report it confidentially.

**Please do NOT report security vulnerabilities through public GitHub issues.**

Report via email to: **security@finops-framework.dev**

You should receive a response within 48 hours. If not, please follow up.

### What to Include

- Description of the vulnerability
- Affected component (discovery script, dashboard, Lambda function, etc.)
- Steps to reproduce
- Potential impact (e.g., exposure of AWS credentials, cost data leakage)

## Security Practices

### Credential Management

- **AWS Credentials:** All scripts and automation use IAM roles or short-lived credentials (STS). Long-lived access keys are never stored in code, configuration files, or version control
- **Least Privilege:** The IAM policies used by this framework grant read-only access to cost and resource data. Write operations (cleanup, resizing) require separate, explicitly authorized roles
- **No Hardcoded Secrets:** API keys, tokens, and passwords are never committed to Git. All secrets are injected via environment variables, AWS Secrets Manager, or Parameter Store

### Data Protection

- **Cost Data:** Cost and usage data fetched from AWS APIs may contain sensitive business information. Data at rest in S3 or the dashboard database is encrypted
- **API Security:** If the framework exposes APIs (e.g., for dashboards), they are secured with authentication and TLS
- **Logging:** Logs are sanitized to remove sensitive information (account IDs, cost details) before storage

### Access Control

- **Dashboard Access:** Grafana dashboards are protected by authentication (OAuth/OIDC recommended). Access to cost views is role-scoped
- **Automation Safety:** Cleanup and resizing actions require explicit approval — no automatic destructive operations without human review
- **Multi-Tenant Isolation:** If used across multiple accounts or teams, data is isolated by tags and access controls

### Script & Code Security

- **Input Validation:** All inputs to scripts and Lambda functions are validated to prevent injection attacks
- **Dependency Scanning:** Python dependencies are scanned for known vulnerabilities using SCA tools
- **IaC Scanning:** Terraform configurations for the framework itself are scanned with Checkov/tfsec for misconfigurations

### AWS API Safety

- **Read-Only by Default:** The framework operates in read-only mode for data collection. Any write operations (cleanup, tag enforcement) require explicit opt-in with elevated permissions
- **Rate Limiting:** API calls respect AWS service quotas and implement exponential backoff
- **Audit Trail:** All actions are logged with timestamp, IAM principal, and resource ARN for traceability

## Responsible Disclosure

We ask that you:

- Give us reasonable time to investigate and fix issues before public disclosure
- Avoid actions that could compromise AWS accounts, expose customer data, or incur costs
- Report issues in good faith

## Recognition

Security researchers who report valid vulnerabilities will be acknowledged (with permission).
