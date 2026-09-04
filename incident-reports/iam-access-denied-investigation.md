# Incident Report: Unauthorized IAM Access Attempt

## Incident Summary

A controlled security test was performed using a restricted development IAM user. The user attempted to access IAM resources without the required permissions.

AWS denied the request, and CloudTrail recorded the activity with an `AccessDenied` error. The event confirmed that the least-privilege controls were operating correctly.

## Incident Details

| Field            | Information                        |
| ---------------- | ---------------------------------- |
| Incident type    | Unauthorized IAM access attempt    |
| AWS service      | AWS Identity and Access Management |
| Detection source | AWS CloudTrail                     |
| Actor type       | Restricted development IAM user    |
| Result           | Access denied                      |
| Error code       | `AccessDenied`                     |
| Severity         | Low                                |
| Classification   | Expected controlled security test  |
| Account impact   | None detected                      |
| Data exposure    | None detected                      |

## Detection

The incident was detected by reviewing AWS CloudTrail Event History. The event record contained an `AccessDenied` error, showing that the development user attempted an IAM action that was not permitted by the attached policy.

## Investigation

The following information was reviewed:

* IAM identity that initiated the request
* Event source
* Attempted API action
* Event time
* AWS Region
* Error code and error message
* Read-only status
* Source IP address
* User agent

Sensitive identifiers were removed from the published evidence.

## Findings

The investigation established that:

* The request originated from the restricted development user used in the lab.
* The user did not possess permission to access IAM user information.
* AWS correctly rejected the request.
* CloudTrail successfully recorded the denied activity.
* No policy modification or unauthorized access occurred.
* No evidence of account compromise was identified.

## Containment and Response

No emergency containment was required because AWS denied the action before access was granted.

The following controls were confirmed:

* Least-privilege permissions restricted the development user.
* IAM administrative access was not assigned to the development group.
* CloudTrail provided evidence of the attempted action.
* MFA protected the privileged administrative identity.

## Security Outcome

This controlled test demonstrated that preventive and detective controls worked together:

1. IAM prevented the unauthorized action.
2. CloudTrail recorded the denied request.
3. The event could be investigated and classified.
4. The activity produced no detected security impact.

## Recommendations

* Continue applying least-privilege permissions.
* Review CloudTrail regularly for repeated `AccessDenied` events.
* Investigate unexpected denied requests from unfamiliar users or locations.
* Maintain MFA for privileged identities.
* Avoid assigning administrative policies to development users.
* Create alerts for suspicious or repeated authorization failures when supported.

## Evidence

![Development user AccessDenied message](../screenshots/12-dev-user-iam-access-denied.png)

![CloudTrail AccessDenied event](../screenshots/13-cloudtrail-access-denied-event.png)

## Conclusion

The event was a successful controlled security test and not a genuine compromise. AWS IAM blocked the unauthorized request, while CloudTrail provided sufficient information for investigation. The incident was closed with no additional remediation required.
