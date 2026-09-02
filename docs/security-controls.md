# Security Controls

This document records the security controls implemented during the AWS Cloud Security Monitoring Lab. Each section explains the objective, configuration, security value, evidence and result of a control.

---

## Control 1: Zero-Spend Budget Alert

### Objective

Create an early-warning mechanism that detects unexpected spending within the AWS account.

Unexpected charges may be caused by accidentally running resources, incorrect service configurations, expired free usage allowances, or unauthorized activity. Receiving an early notification reduces the time between the start of unexpected usage and its discovery.

### AWS Service

AWS Budgets

### Date Implemented

September 2026

### Configuration

| Setting             | Value                       |
| ------------------- | --------------------------- |
| Budget setup        | Template — simplified       |
| Template            | Zero spend budget           |
| Alert threshold     | Spending above $0.01        |
| Notification method | Email                       |
| Budget name         | AWS-Security-Lab-Zero-Spend |
| Status              | Created successfully        |

### Implementation Steps

1. Opened AWS Billing and Cost Management.
2. Navigated to Budgets and Planning.
3. Selected Budgets.
4. Selected Create budget.
5. Chose the simplified budget template.
6. Selected the Zero spend budget template.
7. Entered a descriptive budget name.
8. Added an email recipient for notifications.
9. Created the budget.
10. Confirmed that the budget was active.

### Security Value

The budget provides a basic financial-monitoring control for the AWS account. Although it does not automatically stop resources, it helps identify unexpected usage quickly.

Unexpected cloud spending may indicate:

* A forgotten or incorrectly configured resource
* Activity outside the intended lab scope
* Compromised AWS credentials
* Unauthorized resource creation
* Usage beyond available AWS credits or free allowances

The alert therefore supports both cost management and cloud-security monitoring.

### Result

The zero-spend budget was created successfully. AWS will send an email notification when detected spending exceeds $0.01.

### Evidence

A sanitized screenshot of the active budget will be stored at:

`screenshots/01-zero-spend-budget.png`

Before uploading the screenshot, the following information must be hidden or cropped:

* Email address
* AWS account number
* Personal name
* Billing information
* Any other account-specific identifiers

### Limitations

AWS Budgets can notify the account owner about spending, but it does not automatically prevent services from creating charges. Additional controls, including least-privilege IAM policies, resource monitoring and regular billing reviews, are still required.

### Status

Completed

