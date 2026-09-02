# aws-cloud-security-monitoring-lab
# AWS Cloud Security Monitoring Lab

## Project Overview

This project demonstrates the design and implementation of security monitoring controls in an AWS environment. It focuses on protecting cloud resources, monitoring account activity, detecting suspicious behavior, controlling permissions, and responding to security events.

The lab is designed to provide practical experience with AWS Identity and Access Management (IAM), AWS CloudTrail, Amazon S3 security, IAM Access Analyzer, Amazon GuardDuty, security alerts, and cloud incident response.

## Project Objectives

* Implement AWS cost and usage monitoring.
* Assess IAM users, groups, roles, policies, and access keys.
* Apply the principle of least privilege.
* Monitor account activity with AWS CloudTrail.
* Generate and investigate controlled security events.
* Secure an Amazon S3 bucket.
* Identify publicly or externally accessible resources.
* Detect potential threats with Amazon GuardDuty.
* Create professional security findings and incident reports.
* Document the project for a cybersecurity portfolio.

## Lab Environment

| Component           | Purpose                                   |
| ------------------- | ----------------------------------------- |
| AWS IAM             | Identity and permission management        |
| AWS CloudTrail      | Account activity and API event logging    |
| AWS Budgets         | Unexpected-spending detection             |
| Amazon S3           | Secure cloud storage configuration        |
| IAM Access Analyzer | External and public access identification |
| Amazon GuardDuty    | Threat detection and security findings    |
| GitHub              | Project documentation and evidence        |

## Project Progress

* [ ] Create zero-spend AWS budget alert
* [ ] Perform IAM security assessment
* [ ] Review users, groups, roles and policies
* [ ] Analyze CloudTrail event history
* [ ] Generate controlled allowed and denied events
* [ ] Configure and assess S3 security
* [ ] Review IAM Access Analyzer findings
* [ ] Test GuardDuty threat detection
* [ ] Produce an incident-response report
* [ ] Complete the final security assessment

## Repository Structure

```text
aws-cloud-security-monitoring-lab/
├── README.md
├── docs/
│   ├── security-controls.md
│   ├── security-assessment.md
│   └── lessons-learned.md
├── screenshots/
├── policies/
├── incident-reports/
├── diagrams/
└── LICENSE
```

## Completed Security Controls

### Zero-Spend Budget Alert

A zero-spend AWS Budget was configured to provide an email notification when account spending exceeds $0.01. This provides early warning of unexpected resource usage, configuration mistakes, or potentially unauthorized activity.

Detailed implementation notes are available in `docs/security-controls.md`.

## Security and Privacy

No passwords, access keys, secret keys, MFA codes, account numbers, private keys, personal email addresses, or unredacted sensitive screenshots are stored in this repository.

## Skills Demonstrated

* AWS cloud security
* Identity and access management
* Security monitoring
* Log analysis
* Threat detection
* Incident response
* Cost anomaly awareness
* Technical documentation

## Status

This project is currently in progress.
