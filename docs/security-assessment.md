# AWS Cloud Security Final Assessment

## Executive Summary

This assessment reviews the security controls implemented during the AWS Cloud Security Monitoring Lab.

The project improved identity protection, access control, activity monitoring, secure object storage and security-event investigation. The assessment found that the implemented controls operated as expected during testing.

Amazon GuardDuty could not be tested because it was unavailable under the current AWS Free account plan. The account was not upgraded to avoid unexpected charges.

## Assessment Scope

The following services and controls were assessed:

* AWS Identity and Access Management
* Multi-factor authentication
* IAM users and groups
* Customer-managed IAM policies
* Attribute-based access control
* IAM Policy Simulator
* AWS CloudTrail Event History
* Amazon S3 security
* IAM Access Analyzer
* AWS Budgets
* Incident investigation and documentation
* Amazon GuardDuty availability

## Assessment Methodology

The assessment used:

1. Manual AWS configuration review
2. Before-and-after security comparisons
3. IAM policy inspection
4. Permission simulation
5. Controlled allowed and denied actions
6. CloudTrail event analysis
7. S3 security-configuration review
8. S3 object-version testing
9. External-access analysis
10. Sanitized screenshot evidence

## Findings Summary

| ID   | Finding                                                  | Initial risk | Final status        |
| ---- | -------------------------------------------------------- | ------------ | ------------------- |
| F-01 | Privileged identities lacked MFA                         | High         | Remediated          |
| F-02 | Development policy granted broad `ec2:*` access          | High         | Remediated          |
| F-03 | EC2 instance termination was not explicitly denied       | High         | Remediated          |
| F-04 | Development users could potentially modify resource tags | High         | Remediated          |
| F-05 | Development and production access required separation    | High         | Remediated          |
| F-06 | Cloud activity had not been formally investigated        | Medium       | Remediated          |
| F-07 | Secure cloud-storage controls required validation        | Medium       | Remediated          |
| F-08 | External resource access had not been assessed           | Medium       | Remediated          |
| F-09 | Unexpected cloud spending required early warning         | Medium       | Remediated          |
| F-10 | GuardDuty threat detection could not be enabled          | Medium       | Accepted limitation |

## Detailed Assessment Results

### F-01: Missing Multi-Factor Authentication

**Initial condition:** Privileged identities relied on password-only authentication.

**Risk:** A stolen or compromised password could provide unauthorized console access.

**Remediation:** MFA was enabled and tested for privileged identities.

**Status:** Remediated.

### F-02: Excessive EC2 Permissions

**Initial condition:** The development policy allowed broad `ec2:*` permissions.

**Risk:** A compromised development identity could perform unnecessary or destructive EC2 actions.

**Remediation:** The policy was restricted to:

* `ec2:Describe*`
* `ec2:StartInstances`
* `ec2:StopInstances`
* `ec2:RebootInstances`

**Status:** Remediated.

### F-03: EC2 Termination Risk

**Initial condition:** Instance termination was not explicitly prohibited.

**Risk:** Development users could potentially delete EC2 instances and disrupt workloads.

**Remediation:** An explicit deny was added for `ec2:TerminateInstances`.

**Validation:** IAM Policy Simulator returned an explicit denial.

**Status:** Remediated.

### F-04: Resource-Tag Modification Risk

**Initial condition:** Development users could potentially change tags used for authorization.

**Risk:** A user might modify an environment tag to bypass tag-based access restrictions.

**Remediation:** Explicit denies were configured for:

* `ec2:CreateTags`
* `ec2:DeleteTags`

**Validation:** IAM Policy Simulator confirmed that tag creation was denied.

**Status:** Remediated.

### F-05: Environment Access Separation

**Initial condition:** Development and production resources required permission separation.

**Risk:** Development users could potentially perform operational actions against production resources.

**Remediation:** EC2 management actions were restricted to instances tagged:

```text
Env = development
```

**Validation:** Development-tagged actions were allowed, while production-tagged actions were denied during simulation.

**Status:** Remediated.

### F-06: Cloud Activity Investigation

**Initial condition:** Cloud activity had not been formally reviewed or classified.

**Risk:** Unauthorized or suspicious API activity could remain unnoticed.

**Remediation:** CloudTrail Event History was used to investigate:

* A successful IAM policy-version change
* The creation of policy version `v2`
* A controlled unauthorized IAM request
* An `AccessDenied` event

**Status:** Remediated.

### F-07: Amazon S3 Security

**Initial condition:** A protected cloud-storage resource had not been assessed.

**Risk:** Objects could be exposed, overwritten, transmitted insecurely or managed through outdated ACL permissions.

**Remediation:** The S3 bucket was configured with:

* Bucket Versioning
* SSE-S3 default encryption
* All Block Public Access settings
* Bucket owner enforcement
* Disabled ACLs
* An explicit denial of insecure HTTP transport
* A project-identification tag

Two versions of a harmless test object were uploaded to confirm version retention.

**Status:** Remediated.

### F-08: External Access Assessment

**Initial condition:** Resource policies had not been assessed for external access.

**Risk:** A resource could unintentionally grant access to another AWS account or the public.

**Remediation:** An IAM Access Analyzer external-access analyzer was created with the current account as its zone of trust.

**Result:** The completed scan returned zero active findings. No supported resource was detected as granting access outside the account at the time of assessment.

**Status:** Remediated.

### F-09: Unexpected Cloud Spending

**Initial condition:** No early-warning control existed for unexpected AWS spending.

**Risk:** Accidental resource use or unauthorized activity could create unplanned charges.

**Remediation:** A zero-spend AWS Budget notification was configured.

**Status:** Remediated.

### F-10: GuardDuty Availability

**Initial condition:** Amazon GuardDuty threat-detection testing was included in the project plan.

**Finding:** GuardDuty was unavailable under the current AWS Free account plan.

**Decision:** The account was not upgraded because doing so could introduce chargeable usage.

**Risk:** Automated managed threat detection was not validated during this project.

**Recommendation:** Enable GuardDuty during a future controlled assessment when paid-plan access and billing monitoring are available.

**Status:** Accepted limitation.

## Control Validation Results

| Security control             | Validation result          |
| ---------------------------- | -------------------------- |
| MFA protection               | Enabled and verified       |
| Least-privilege EC2 access   | Passed                     |
| Development-tag access       | Passed                     |
| Production-tag restriction   | Passed                     |
| Termination protection       | Explicitly denied          |
| Tag-modification protection  | Explicitly denied          |
| CloudTrail event recording   | Passed                     |
| AccessDenied event detection | Passed                     |
| S3 Block Public Access       | Enabled                    |
| S3 default encryption        | Enabled                    |
| HTTPS-only S3 access         | Enforced                   |
| S3 object versioning         | Enabled and tested         |
| External-access analysis     | Zero active findings       |
| Budget notification          | Configured                 |
| GuardDuty testing            | Not available on Free plan |

## Residual Risks

The following risks remain:

* GuardDuty automated threat detection was not tested.
* CloudTrail Event History requires manual review.
* No automated alert was configured for repeated authorization failures.
* The project represents a single-account lab and not a production multi-account environment.
* Security configurations must continue to be reviewed as resources and policies change.

## Recommendations

1. Keep MFA enabled for every privileged identity.
2. Review IAM permissions regularly.
3. Remove unused users, roles, policies and access keys.
4. Continue using groups instead of assigning policies directly to individual users.
5. Review CloudTrail for unexpected policy changes and repeated access failures.
6. Preserve S3 Block Public Access and HTTPS enforcement.
7. Review Access Analyzer whenever resource policies change.
8. Monitor the AWS Budget notification and billing dashboard.
9. Enable GuardDuty when appropriate billing controls and service access are available.
10. Avoid publishing unsanitized AWS evidence.

## Overall Conclusion

The AWS environment demonstrated a strong improvement from its initial state. High-risk IAM weaknesses were remediated, privileged access was protected with MFA, EC2 permissions were restricted, CloudTrail activity was investigated and the S3 bucket was secured using layered controls.

IAM Access Analyzer returned zero external-access findings, and the controlled unauthorized-access test confirmed that IAM prevention and CloudTrail detection worked together successfully.

The overall security posture for the lab is assessed as **improved and suitable for the intended learning environment**, with GuardDuty testing and automated alerting retained as future improvements.

This assessment does not represent a formal compliance certification or guarantee that the AWS account is free from every security risk.
