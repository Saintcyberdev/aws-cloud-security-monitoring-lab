# Lessons Learned

## Overview

This project provided practical experience in securing, monitoring and assessing an AWS environment.

The most important lesson was that cloud security does not depend on one service or one configuration. Effective security requires multiple preventive, detective, recovery and administrative controls working together.

## 1. Identity Is a Primary Security Boundary

AWS IAM determines who can access the environment and what each identity can do.

During the initial assessment, privileged identities were protected only by passwords. Enabling MFA demonstrated that strong authentication should be applied before expanding cloud usage.

I learned that:

* Privileged accounts require stronger protection than passwords alone.
* MFA reduces the risk created by stolen or compromised passwords.
* Separate administrative and development identities help reduce unnecessary privilege.
* Permissions should normally be assigned through groups or roles instead of directly to individual users.
* Unused users, roles and access keys should be reviewed and removed.

## 2. Least Privilege Requires Specific Permissions

The original development policy allowed broad `ec2:*` access. Although the policy included a tag condition, the wildcard still provided more permissions than the user required.

The remediated policy allowed only:

* Viewing EC2 resources
* Starting development instances
* Stopping development instances
* Rebooting development instances

The policy explicitly denied instance termination and tag modification.

I learned that least privilege means identifying the exact actions required for a job instead of granting an entire service.

## 3. Explicit Denies Provide Strong Protection

An explicit deny was used for:

* `ec2:TerminateInstances`
* `ec2:CreateTags`
* `ec2:DeleteTags`

This demonstrated that an explicit deny takes priority over an allow when AWS evaluates permissions.

The termination deny protects resources from destructive actions. The tag-modification deny prevents users from changing the tags used to make authorization decisions.

## 4. Tags Can Be Used for Access Control

The policy restricted EC2 management actions to resources tagged:

```text
Env = development
```

This demonstrated attribute-based access control.

Development-tagged resources could be managed, while production-tagged resources were denied during simulation.

I learned that tag-based permissions can help separate environments, but the tags must also be protected. If users can freely change authorization tags, they may be able to bypass the intended restriction.

## 5. Policies Should Be Tested Before Real-World Use

IAM Policy Simulator made it possible to validate the policy without performing destructive actions against actual resources.

The simulator confirmed that:

* Viewing EC2 resources was allowed.
* Development management actions were allowed.
* Production management actions were denied.
* Instance termination was explicitly denied.
* Tag creation was explicitly denied.

I learned that policy testing should include both expected allowed actions and expected denied actions.

A policy is not fully validated simply because one approved action succeeds.

## 6. CloudTrail Provides Evidence, Not Automatic Conclusions

CloudTrail recorded AWS API and management activity, including:

* IAM policy-version changes
* Policy Simulator activity
* Authorized configuration changes
* Unauthorized requests
* `AccessDenied` errors

I learned how to review:

* User identity
* Event name
* Event source
* Event time
* AWS Region
* Source IP address
* User agent
* Request parameters
* Response information
* Error code and error message
* Read-only status

A CloudTrail event provides evidence, but an analyst must interpret the evidence using context.

## 7. An AccessDenied Event Is Not Automatically an Attack

The controlled development user received an `AccessDenied` error after attempting an unauthorized IAM action.

The event showed that the preventive IAM control was working. Because the action was an intentional lab test, it was classified as expected activity rather than a genuine compromise.

I learned that an analyst should ask:

* Was the action expected?
* Was the user authorized?
* Was the source familiar?
* Did the request succeed?
* Were related actions performed?
* Was any resource changed?
* Was any information exposed?

Unexpected or repeated denied requests may still indicate credential misuse, reconnaissance, a misconfigured application or an attempted attack.

## 8. Incident Response Must Match the Evidence

The controlled incident did not require emergency containment because AWS had already denied the action.

The appropriate response was to:

* Verify the identity
* Confirm the action was denied
* Review the applicable permissions
* Check for related events
* Determine the impact
* Preserve sanitized evidence
* Document the investigation
* Close the event as a controlled test

I learned that incident response should be based on the evidence and severity. Not every security event requires disabling accounts or deleting resources.

## 9. S3 Security Requires Multiple Layers

The S3 bucket was protected using several controls:

* SSE-S3 default encryption
* Block Public Access
* Bucket owner enforcement
* Disabled ACLs
* HTTPS enforcement
* Bucket Versioning
* Project tags

Each control addresses a different risk.

| S3 control            | Security purpose                               |
| --------------------- | ---------------------------------------------- |
| SSE-S3                | Encrypts objects stored in the bucket          |
| Block Public Access   | Prevents accidental public exposure            |
| Bucket owner enforced | Centralizes object ownership                   |
| Disabled ACLs         | Removes outdated ACL-based access management   |
| HTTPS enforcement     | Prevents insecure transport                    |
| Versioning            | Supports recovery from overwriting or deletion |
| Tags                  | Supports organization and identification       |

I learned that encryption alone does not make a bucket secure. Access control, transport security, ownership and recovery must also be considered.

## 10. Versioning Supports Recovery

Two versions of the same harmless test object were uploaded.

S3 retained both versions, demonstrating that an overwritten object does not necessarily destroy the previous content when versioning is enabled.

I learned that versioning improves recovery but does not replace backups. Versions still require access controls, lifecycle management and protection from unauthorized deletion.

## 11. Access Analyzer Identifies External Trust

IAM Access Analyzer was configured with the current AWS account as the zone of trust.

The scan returned zero active findings. This indicated that no supported resource was detected as granting public or external-account access at the time of the assessment.

I learned that zero findings does not prove the entire account is completely secure. It means that the analyzer did not identify external access within its supported scope and current configuration.

The analyzer should be reviewed again whenever resource policies change.

## 12. Cost Monitoring Is Part of Cloud Security

A zero-spend AWS Budget alert was configured before expanding the lab.

This provides warning of unexpected spending caused by:

* Forgotten resources
* Configuration errors
* Unintended usage
* Potentially unauthorized activity

I learned that unexpected costs can be an operational warning sign and that security testing should consider financial risk.

## 13. Service Limitations Must Be Documented Honestly

Amazon GuardDuty could not be tested because it was unavailable under the current AWS Free account plan.

The account was not upgraded because avoiding unexpected charges was an important project requirement.

I learned that a professional assessment should clearly identify:

* What was tested
* What was not tested
* Why testing was not possible
* What risk remains
* What should be completed later

A documented limitation is more accurate and professional than claiming that an unavailable control was tested.

## 14. Evidence Must Be Sanitized

AWS screenshots and event records can contain sensitive information, including:

* AWS account numbers
* Usernames
* Access-key identifiers
* Principal IDs
* IP addresses
* Email addresses
* Resource ARNs
* Event IDs
* Request IDs
* MFA information

I learned to crop or redact this information before uploading evidence to a public GitHub repository.

Screenshots should prove that a control was implemented without exposing information that could assist an attacker.

## 15. Documentation Is Part of the Security Work

The project documentation includes:

* A project overview
* Architecture and monitoring flow
* Security-control implementation notes
* Sanitized screenshots
* IAM policy information
* CloudTrail investigations
* An incident report
* A final security assessment
* Lessons learned

I learned that screenshots alone do not demonstrate analytical ability. Professional documentation should explain what was configured, why it matters, how it was tested and what the results mean.

## Challenges Encountered

The main challenges included:

* Understanding the relationship between IAM users, groups and policies
* Replacing broad permissions with specific actions
* Configuring policy conditions correctly
* Interpreting IAM Policy Simulator results
* Distinguishing successful and denied CloudTrail events
* Removing sensitive information from screenshots
* Ensuring screenshot filenames matched Markdown links
* Understanding the different S3 security settings
* Working within AWS Free-plan service limitations

Resolving these challenges improved both technical knowledge and troubleshooting ability.

## Future Improvements

Future versions of the project could include:

* Amazon GuardDuty sample-finding analysis
* Automated alerts for repeated `AccessDenied` events
* EventBridge security-event routing
* Amazon SNS email notifications
* Centralized CloudTrail logging
* Multi-account security monitoring
* IAM Access Analyzer policy validation
* Automated configuration deployment using Terraform or CloudFormation
* S3 lifecycle policies for older object versions
* Additional incident-response scenarios

These improvements should be performed only after reviewing pricing and establishing appropriate billing controls.

## Final Reflection

This project strengthened my understanding of how AWS preventive and detective controls work together.

IAM and S3 controls prevented unauthorized or insecure access. CloudTrail and IAM Access Analyzer provided visibility. Policy simulation allowed permissions to be tested safely. Versioning supported recovery, while the budget alert reduced financial risk.

The project also demonstrated that cloud security involves more than enabling services. It requires understanding risk, applying suitable controls, testing expected behavior, investigating evidence, documenting limitations and communicating findings clearly.
