# DevSecOps Portfolio — Linux Security Hardening

## Overview

This project demonstrates a basic Linux security auditing workflow as part of a DevSecOps portfolio. It uses Bash scripting to inspect system information and check the availability and status of selected security controls.

## Objectives

- Audit basic Linux security configuration.
- Check SSH service status.
- Inspect UFW firewall status.
- Check Fail2ban availability.
- Document findings and remediation recommendations.

## Technologies

- Ubuntu Linux
- Bash scripting
- Git and GitHub
- GitHub Actions

## Project Structure

```text
devsecops-portfolio/
├── README.md
├── scripts/
│   └── linux-hardening.sh
├── docs/
│   └── security-report.md
├── screenshots/
└── .github/
    └── workflows/
        └── security-audit.yml
```

## Run the Audit

```bash
chmod +x scripts/linux-hardening.sh
./scripts/linux-hardening.sh
```

Some checks may report unavailable services depending on the execution environment.

## Security Scope

This script performs basic checks only. It does not automatically remediate system settings or guarantee that the host is secure.

## Future Improvements

- Automated shell script linting with ShellCheck.
- Security checks in GitHub Actions.
- Container vulnerability scanning with Trivy.
- Docker image hardening.
- Kubernetes security controls.
- GitOps deployment with Argo CD.

## Author

DevSecOps Portfolio Project
