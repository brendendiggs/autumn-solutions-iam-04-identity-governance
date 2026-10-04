# Identity Governance

## Autumn Solutions

This project demonstrates governed, time-bound access to Finance resources using Microsoft Entra Entitlement Management.

The business requirement was to ensure that access was not granted manually or indefinitely. Instead, Finance access had to follow a controlled identity governance lifecycle:

**Request -> Justification -> Approval -> Provisioning -> Access Review -> Revocation**


## Video Walkthrough

▶ **[Watch the 2–3 minute project walkthrough](https://youtu.be/mKYtYUfVH1c)**

See the access request, approval, provisioning, access review, and governance-driven revocation workflow demonstrated end to end.

## Business Scenario

Employees may occasionally require temporary access to Finance systems.

Autumn Solutions requires that this access:

- Be requested through a governed workflow
- Include a business justification
- Require approval from an authorized reviewer
- Automatically provision approved resources
- Be time-bound
- Be periodically reviewed
- Be automatically removed when no longer justified

## Technologies

- Microsoft Entra ID
- Microsoft Entra Entitlement Management
- Microsoft Entra Access Packages
- Microsoft Entra Access Reviews
- Microsoft Entra My Access
- Enterprise Applications
- Security Groups
- Identity Governance

## Governance Architecture

The governance workflow is:

**Catalog -> Access Package -> Request Policy -> Approval -> Resource Assignment -> Access Review -> Retain or Remove**

The architecture separates access request, approval, provisioning, and review responsibilities instead of relying on direct administrative assignment.

## Catalog

### Finance Access Governance

A dedicated entitlement management catalog was created for governed Finance access.

The catalog contains the resources required for the Finance access package.

### Governed Resources

- `SG-Finance`
- `LedgerFlow`

These resources represent both group-based access and application access.

## Access Package

### Finance Systems Access

The **Finance Systems Access** package provides governed access to:

- Finance security group membership
- LedgerFlow enterprise application access

Using an access package allows both resources to be governed through a single request and lifecycle process.

## Request Policy

The request policy was configured so that eligible users can request Finance access through Microsoft Entra My Access.

The workflow requires:

- Self-service request
- Requester justification
- Required business-process question
- One-stage approval
- Approver justification
- Time-bound assignment

### Required Business Question

The requester must answer:

**Which Finance business process requires this access?**

This creates additional context for the approver instead of relying only on a generic access request.

## Approval Workflow

Finance access requires approval from:

**Jordan Lee**

The approval configuration includes:

- One approval stage
- Three-day approval deadline
- Approver justification required

This provides separation between the person requesting access and the person authorizing it.

## Assignment Lifecycle

Approved access is configured with:

- 30-day assignment duration
- Extension permitted with approval
- Required access review
- Monthly review recurrence
- Seven-day review duration
- Jordan Lee as reviewer
- Reviewer justification required
- Review reminders enabled
- Access removal when continued access is not approved

This prevents Finance access from becoming permanent simply because it was approved once.

## Live Governance Test

A real end-to-end test was performed using:

**Requester:** Taylor Brooks  
**Approver:** Jordan Lee

Taylor Brooks requested the **Finance Systems Access** package through My Access.

### Business Process Response

Taylor entered:

**Month-end reconciliation and expense reporting**

### Request Justification

Taylor supplied justification for temporary Finance access.

The request entered a pending approval state rather than immediately granting access.

This validated that the policy was enforcing the approval control.

## Approval Validation

Jordan Lee reviewed the request and approved it.

The approval justification was:

**Approved for temporary Finance support duties. Access is limited to the governed access package duration.**

This demonstrated that authorization was performed by the designated approver rather than by the requester or IAM administrator.

## Automatic Provisioning

After Jordan approved the request, Microsoft Entra Entitlement Management automatically provisioned the resources contained in the access package.

Taylor Brooks received:

- Membership in `SG-Finance`
- Access to the `LedgerFlow` enterprise application

Neither resource was manually assigned to Taylor.

This validates one of the most important controls in the project:

**The governance decision drives provisioning automatically.**

## Access Review Configuration

The assignment is governed by an access review.

The review configuration includes:

- Monthly recurrence
- Seven-day review window
- Jordan Lee as reviewer
- Reviewer justification required
- Decision helpers enabled
- Reminders enabled
- Automatic removal when access is not approved

The first review was scheduled to begin after the initial governed assignment was created.

The configured review activation time is approximately:

**September 6, 2026 at 11:59 PM Eastern Time**

The review must become active before the reviewer can submit the final governance decision.

## Final Governance Validation

The final stage of the project validates governance-driven revocation.

The workflow is:

1. Access review becomes active
2. Jordan Lee reviews Taylor Brooks
3. Jordan determines that continued Finance access is no longer required
4. Jordan denies continued access and provides justification
5. Microsoft Entra applies the review result
6. Taylor is automatically removed from `SG-Finance`
7. Taylor automatically loses `LedgerFlow` access

No manual group removal or application removal is used.

This validates that access removal is driven by the governance lifecycle rather than by an administrator manually cleaning up access.

## Governance Controls Demonstrated

This project demonstrates:

- Self-service access requests
- Business justification
- Custom governance questions
- Separation of duties
- Approval workflows
- Approver justification
- Access packages
- Entitlement catalogs
- Automatic resource provisioning
- Time-bound access
- Access reviews
- Reviewer accountability
- Automatic access revocation
- Least privilege
- Delegated identity governance

## Portfolio Evidence

Evidence captured during the completed governance lifecycle:

1. `01-Taylor-Request-Details-Pending-Approval.png`
   - Taylor Brooks submitted a governed request for Finance access with business justification.

2. `02-Jordan-Approval-Approved.png`
   - Jordan Lee approved the temporary Finance access request.

3. `03-Taylor-SG-Finance-Access-Granted.png`
   - Entitlement Management automatically provisioned Taylor into SG-Finance.

4. `04-Taylor-LedgerFlow-Access-Granted.png`
   - Entitlement Management automatically provisioned Taylor to LedgerFlow.

5. `05-Access-Review-Configured-Not-Started.png`
   - The recurring access review was configured before activation.

6. `06-Jordan-Access-Review-Active.png`
   - The scheduled access review activated automatically.

7. `07-Jordan-Access-Review-Taylor-Pending-Decision.png`
   - Taylor appeared as the pending review subject for Jordan.

8. `08-Jordan-Access-Review-Denied.png`
   - Jordan denied continued Finance access.

9. `09-Admin-Access-Review-Denied-Result-Active.png`
   - The admin view confirmed one denied user in the active review.

10. `10-Admin-Access-Review-Taylor-Denied-Result.png`
    - The detailed result showed Taylor denied despite an Entra recommendation to approve.

11. `11-Access-Review-Audit-Log-Auto-Apply-Success.png`
    - Audit logs showed the deny decision, review completion, and automatic review-processing events succeeding.

12. `12-Access-Review-Results-Applied.png`
    - The completed review instance reached Results applied.

13. `13-Taylor-Finance-Access-Package-Expired.png`
    - Taylor's governed assignment transitioned from Delivered to Expired.

14. `14-Taylor-SG-Finance-Access-Revoked.png`
    - Taylor was automatically removed from SG-Finance.

15. `15-Taylor-LedgerFlow-Access-Revoked.png`
    - Taylor was automatically removed from LedgerFlow.


16. `16-IAM-04-Public-Portfolio-Case-Study.png`
    - Public recruiter-facing IAM-04 case study published at brendendiggs.com.

The evidence demonstrates the complete lifecycle:

**request -> approval -> automated provisioning -> access review -> denial -> results applied -> automated revocation**

No manual group membership or enterprise-application removal was used.

## Security Design Decisions

### No Manual Provisioning

Taylor was not manually added to the Finance group or LedgerFlow application.

Entitlement Management performed provisioning only after the governed request was approved.

### Separation of Duties

Taylor requested access.

Jordan approved and reviews access.

The IAM administrator configured the governance controls but did not make the business access decision.

### Time-Bound Access

Finance access is not permanent.

Assignments have a defined lifecycle and require continued justification.

### Governance-Driven Revocation

The final control is designed so that access removal is triggered by the access review decision rather than manual administrator intervention.

### Business Context

Both requester and approver justifications are captured to provide evidence explaining why privileged business access was granted.

## Key Outcomes

This project demonstrates how Microsoft Entra Identity Governance can control the complete lifecycle of sensitive access.

The solution combines:

- Request governance
- Approval governance
- Automated provisioning
- Time-bound access
- Periodic certification
- Automated revocation

Instead of treating access as a one-time administrative action, Autumn Solutions manages Finance access as a governed lifecycle from initial request through eventual removal.
