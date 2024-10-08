# Policies

- IAM policies xác định permissions cho 1 action bất kể method mà được sử dụng để thực hiện action

## Policy types

- **Identity-based policies**: attach các policies đc quản lý bởi IAM identities (users, groups mà user thuộc về, or roles). Identity-based policies grant permissions to an identity
- **Resource-based policies**: attach inline policies to resources. Resource-based policies grant permissions to a principal entity that is specified in the policy. Principals can be in the same account as the resource or in other accounts
- **Permissions boundaries**: use a managed policy as the permissions boundary for an IAM entity (user or role). policy này chỉ định maximum permissions rằng identity-based policies có thể grant cho entity, nhưng không grant permissions. Permissions boundaries ko xác định maximum permissions để 1 resource-based policy có thể grant cho entity
- **Organizations SCPs**: sử dụng AWS Organizations service control policy (SCP) để chỉ định maximum permissions cho account members của 1 organization or organizational unit (OU). SCPs limit permissions để identity-based policies or resource-based policies grant cho entities (users or roles) bên trong account,nhưng không grant permissions
- **Access control lists (ACLs)**: use ACLs to control để principals trong accounts khác có thể access vào resource nơi ACL is attached. ACLs tương tự như resource-based policies, mặc dù chúng chỉ là policy type không sủ dụng JSON policy document structure. ACLs là cross-account permissions policies để grant permissions to cho principal entity đc chỉ định. ACLs không thể grant permissions cho entities trong cùng 1 account
- **Session policies**: chuyển session policies nâng cao khi sử dụng AWS CLI or AWS API để assume 1 role or a federated user. Session policies limit the permissions để role or user's identity-based policies grant đến session. Session policies limit permissions cho 1 session đã tạo, but nhưng ko grant permissions. For more information, see Session Policies

## Policies Deep Dive

- Anatomy of a policy: JSON document with `Effect`, `Action`, `NotAction` (inverse condition of `Action`), `Resource`, `Conditions` and `Policy Variables`
- Priority order of permissions in AWS is: deny (explicit) > allow > deny (implicit). A policy always assumes a default (implicit) deny => if we do not allow explicitly to do something, we wont be able to do it
- An explicit `DENY` has always precedence over `ALLOW`
- Best practice: use least privilege for maximum security
    - Access Advisor: a tool for seeing permissions granted and when last accessed
    - Access Analyzer: used of analyze resources shared with external entities
- Common Managed Policies:
    - `AdministratorAccess`
    - `PowerUserAccess`: does not allow anything regarding to IAM, organizations and account (with some exceptions), otherwise similar to admin access
- IAM policy condition:

    ```
    "Condition": {
        "{condition-operator}": {
            "{condition-key}": "{condition-value}"
        }
    }
    ```

- Operators:
    - String: `StringEquals`, `StringNotEquals`, `StringLike`, etc.
    - Numeric: `NumericEquals`, `NumericNotEquals`, `NumericLessThan`, etc.
    - Date: `DateEquals`, `DateNotEquals`, `DateLessThan`, etc.
    - Boolean
    - IpAddress/NotIpAddress:
        - `"Condition": {"IpAddress": {"aws:SourceIp": "192.168.0.1/16"}}`
    - ArnEquals/ArnLike
    - Null
        - `"Condition": {"Null": {"aws:TokenIssueTime": "192.168.0.1/16"}}`
- Policy Variables and Tags:
    - `${aws:username}`: example `"Resource:["arn:aws:s3:::mybucket/${aws:username}/*"]`
    - AWS Specific:
        - `aws:CurrentTime`
        - `aws:TokenIssueTime`
        - `aws:PrincipalType`: indicates if the principal is an account, user, federated or assumed role
        - `aws:SecureTransport`
        - `aws:SourceIp`
        - `aws:UserId`
    - Service Specific:
        - `ec2:SourceInstanceARN`
        - `s3:prefix`
        - `s3:max-keys`
        - `sns:EndPoint`
        - `sns:Protocol`
    - Tag Based:
        - `iam:ResourceTag/key-name`
        - `iam:PrincipalTag/key-name`

## Permission Boundaries

- Only IDENTITY permissions bị ảnh hưởng bởi boundaries - bất kì resource policies đc apply full
- Permission boundaries can be applied to IAM Users and IAM Roles
- Permission boundaries không grant access cho bất kì action. chúng xác định maximum permissions 1 identity có thể nhận đc
- Use cases for permission boundaries:
    - Delegation problem(vấn đề phân quyền): nếu cấp permissions nâng cao cho 1 user, use đó có thể thăng cấp để có administrator permissions hoặc tạo user khác có administrator permissions
    - Solution là có 1 boundary để cấm tay đổi own user's permissions và cấm tạo users/roles khác elevated permissions nâng cao

## Policy Evaluation Logic

- Components involved in a policy evaluations:
    - Organization SCPs
    - Resource Policies
    - IAM Identity Boundaries
    - Session Policies
    - Identity Policies
- Policy evaluation logic - same account:
    ![policy evaluation logic - same account](images/PolicyEvaluation1.png)
- Policy evaluation logic - different account:
    ![policy evaluation logic - different account](images/PolicyEvaluation2.png)
