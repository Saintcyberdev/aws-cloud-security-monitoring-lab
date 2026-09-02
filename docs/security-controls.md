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

![Zero-spend AWS budget alert](../screenshots/01-zero-spend-budget.png.jpeg)

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



---

## Control 2: IAM Security Assessment and MFA Remediation

### Objective

Assess the AWS account’s IAM configuration, identify authentication weaknesses and strengthen console access by enabling multi-factor authentication.

### AWS Service

AWS Identity and Access Management (IAM)

### Initial IAM Resources

| Resource                  | Quantity |
| ------------------------- | -------: |
| IAM users                 |        2 |
| User groups               |        1 |
| Roles                     |        3 |
| Customer-managed policies |        1 |
| Identity providers        |        0 |

### Initial Security Findings

The IAM Dashboard displayed two security recommendations:

1. MFA was not enabled for the AWS root user.
2. MFA was not enabled for the current IAM user.

The credential report also showed:

| Security check    | Result |
| ----------------- | ------ |
| Active access key | False  |
| MFA enabled       | False  |

The absence of an active access key reduces the risk of programmatic credentials being leaked. However, the absence of MFA means the IAM user’s console access depends only on a password.

### Risk

If the IAM password is exposed through phishing, credential reuse or another compromise, an attacker may be able to access the AWS account.

MFA adds a second authentication factor and reduces the likelihood that a stolen password alone can be used successfully.

### Remediation

MFA was configured for the IAM user using a virtual authenticator application.

The following actions were performed:

1. Reviewed the IAM Dashboard security recommendations.
2. Generated and examined the IAM credential report.
3. Confirmed that the IAM user had no active access key.
4. Selected the IAM recommendation to add MFA.
5. Registered a virtual authenticator application.
6. Verified the MFA registration using authentication codes.
7. Signed out and signed back in to test the new authentication control.
8. Reviewed the IAM Dashboard to confirm that the user-MFA recommendation was removed.

### Evidence Before Remediation

The following screenshot shows the IAM Dashboard before MFA was enabled. The IAM username and account-specific information were removed.

![IAM Dashboard before MFA](../screenshots/02-iam-dashboard-before-mfa.png)

### Evidence After Remediation

The following screenshot shows the updated IAM Dashboard after MFA was enabled.

### Evidence Before Remediation

The following screenshot shows the IAM Dashboard before MFA was enabled. The IAM username and account-specific information were removed.

![IAM Dashboard before MFA](../screenshots/02-iam-dashboard-before-mfa.png.jpeg)

[View the before-MFA evidence](../screenshots/02-iam-dashboard-before-mfa.png.jpeg)

### Evidence After Remediation

The following screenshot shows the updated IAM Dashboard after MFA was enabled.

![IAM Dashboard after MFA](../screenshots/03-iam-dashboard-after-mfa.png.jpeg)

[View the after-MFA evidence](../screenshots/03-iam-dashboard-after-mfa.png.jpeg)

### Result

MFA was successfully enabled for the IAM user. The account now requires both the IAM password and a temporary authentication code during sign-in.

The root-user MFA recommendation remains a separate security finding and will require remediation using the root account.

### Security Improvement

| Before                            | After                                     |
| --------------------------------- | ----------------------------------------- |
| Password-only IAM authentication  | Password and MFA authentication           |
| Two IAM security recommendations  | One IAM security recommendation remaining |
| Higher risk from stolen passwords | Reduced account-takeover risk             |

### Sensitive Information Handling

The MFA QR code, authentication codes, AWS account number, IAM username, credential report and sign-in URL were not uploaded to this repository.

### Status

Completed
