# AWS IAM — Comprehensive Notes

## 1. What is AWS IAM?

**IAM** stands for **Identity and Access Management**.

AWS IAM is a service that helps you securely control access to AWS services and resources.

In simple words:

> IAM decides who can access AWS and what they are allowed to do.

IAM is used to manage:

* Users
* Groups
* Roles
* Policies
* Permissions
* Access keys
* Multi-factor authentication
* Access control for AWS services

Example:

```text
A user wants to start an EC2 instance.

IAM checks:
1. Who is the user?
2. Is the user authenticated?
3. Does the user have permission to start EC2?
4. Is the action allowed or denied?
```

---

## 2. Why Do We Need IAM?

IAM is important because AWS accounts can contain many sensitive resources.

Examples:

* EC2 instances
* S3 buckets
* RDS databases
* Lambda functions
* VPC resources
* Billing information
* CloudWatch logs

Without IAM, everyone could access everything in the AWS account, which is not secure.

IAM helps to:

* Control user access
* Give only required permissions
* Protect sensitive resources
* Manage application permissions
* Apply security best practices
* Avoid using the root account for daily work

---

## 3. Authentication vs Authorization

IAM mainly deals with two important security concepts:

| Concept        | Meaning                                 |
| -------------- | --------------------------------------- |
| Authentication | Verifying who the user is               |
| Authorization  | Checking what the user is allowed to do |

### Authentication Example

```text
User enters username and password.
AWS checks whether the login details are correct.
```

### Authorization Example

```text
User tries to delete an S3 bucket.
IAM checks whether the user has permission to delete that bucket.
```

Simple explanation:

```text
Authentication = Who are you?
Authorization = What are you allowed to do?
```

---

## 4. Root User

When you create an AWS account, AWS creates a **root user**.

The root user has full access to everything in the AWS account.

Root user credentials usually include:

* Root email address
* Root password
* MFA device, if configured

### Important Point

The root user should not be used for daily work.

Use the root user only for account-level tasks that require root access.

Examples:

* Changing account settings
* Closing the AWS account
* Managing root MFA
* Certain billing or support actions

### Best Practice

```text
Enable MFA for root user.
Do not use root user for daily AWS work.
Create IAM users or roles for normal work.
```

---

## 5. IAM Identities

IAM identities are objects used to identify users or workloads in AWS.

Main IAM identities:

```text
IAM Identities
├── IAM User
├── IAM Group
└── IAM Role
```

---

## 6. IAM User

An **IAM user** represents a person or application that needs access to AWS.

Example users:

```text
admin-user
developer-user
student-user
application-user
```

An IAM user can have:

* Console password
* Access keys
* MFA device
* Permissions through policies
* Group membership

### Example

```text
User: John
Needs access to S3 only
Attach S3 read-only policy to John
```

---

## 7. IAM Group

An **IAM group** is a collection of IAM users.

Instead of assigning permissions to each user one by one, you can assign permissions to a group.

Example:

```text
Group: Developers
Users:
- user1
- user2
- user3

Policy attached to group:
- Access to EC2
- Access to CloudWatch
```

If a new developer joins, add the user to the Developers group.

### Benefits of IAM Groups

* Easier permission management
* Consistent access control
* Better organization
* Less repetitive work

### Example Groups

| Group         | Permissions                    |
| ------------- | ------------------------------ |
| Admins        | Full administrative access     |
| Developers    | Access to development services |
| ReadOnlyUsers | Read-only access               |
| BillingTeam   | Billing access                 |
| S3Users       | Access to selected S3 buckets  |

---

## 8. IAM Role

An **IAM role** is an identity with permissions, but it is not permanently assigned to one person.

A role is assumed temporarily by:

* AWS service
* IAM user
* Application
* External account
* Federated user

In simple words:

> A role is a temporary permission set that someone or something can use.

### Example

An EC2 instance needs to access an S3 bucket.

Bad method:

```text
Store AWS access key inside EC2 instance
```

Better method:

```text
Attach IAM role to EC2 instance
EC2 gets temporary credentials automatically
```

### Common Uses of IAM Roles

| Role Type          | Example                                     |
| ------------------ | ------------------------------------------- |
| EC2 role           | EC2 reads files from S3                     |
| Lambda role        | Lambda writes logs to CloudWatch            |
| Cross-account role | User from Account A accesses Account B      |
| Service role       | AWS service performs actions on your behalf |

---

## 9. IAM Policy

An **IAM policy** is a document that defines permissions.

Policies are usually written in JSON format.

A policy says:

```text
Allow or deny specific actions on specific resources.
```

Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-bucket"
    }
  ]
}
```

This policy allows listing one S3 bucket.

---

## 10. Main Parts of an IAM Policy

An IAM policy usually contains these elements:

```text
Policy
├── Version
├── Statement
│   ├── Effect
│   ├── Action
│   ├── Resource
│   └── Condition
```

---

## 11. Version

The `Version` element defines the policy language version.

Common version:

```json
"Version": "2012-10-17"
```

This does not mean the policy was created in 2012. It means the policy uses the current IAM policy language version.

---

## 12. Statement

The `Statement` element contains one or more permission statements.

Example:

```json
"Statement": [
  {
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-bucket/*"
  }
]
```

A policy can have multiple statements.

---

## 13. Effect

The `Effect` element says whether the policy allows or denies access.

Possible values:

```text
Allow
Deny
```

Example:

```json
"Effect": "Allow"
```

or

```json
"Effect": "Deny"
```

Important:

> An explicit Deny always overrides Allow.

---

## 14. Action

The `Action` element defines what action is allowed or denied.

Examples:

```text
s3:GetObject
s3:PutObject
ec2:StartInstances
ec2:StopInstances
rds:CreateDBInstance
lambda:InvokeFunction
```

Example:

```json
"Action": "ec2:StartInstances"
```

Multiple actions:

```json
"Action": [
  "ec2:StartInstances",
  "ec2:StopInstances"
]
```

All actions for a service:

```json
"Action": "s3:*"
```

Be careful with `*`, because it gives broad permissions.

---

## 15. Resource

The `Resource` element defines which AWS resource the action applies to.

Example S3 bucket:

```json
"Resource": "arn:aws:s3:::my-bucket"
```

Example S3 objects inside a bucket:

```json
"Resource": "arn:aws:s3:::my-bucket/*"
```

Example all resources:

```json
"Resource": "*"
```

Using `"Resource": "*"` means the permission applies to all matching resources.

---

## 16. Condition

The `Condition` element adds extra rules for when the policy applies.

Examples:

* Allow access only from a specific IP address
* Require MFA
* Allow access only in a specific region
* Allow access only during certain conditions

Example condition using MFA:

```json
"Condition": {
  "Bool": {
    "aws:MultiFactorAuthPresent": "true"
  }
}
```

---

## 17. Principal

The `Principal` element identifies who is allowed or denied access.

It is mainly used in **resource-based policies**.

Example:

```json
"Principal": {
  "AWS": "arn:aws:iam::123456789012:user/Alice"
}
```

In simple words:

```text
Principal = Who receives the permission?
```

Examples of principals:

* AWS account
* IAM user
* IAM role
* AWS service
* Federated user

---

## 18. Types of IAM Policies

There are several types of policies in AWS.

```text
IAM Policies
├── Identity-based policies
├── Resource-based policies
├── Permissions boundaries
├── Service control policies
├── Session policies
└── Access control lists
```

---

## 19. Identity-Based Policy

An identity-based policy is attached to an IAM identity.

It can be attached to:

* IAM user
* IAM group
* IAM role

Example:

```text
Attach EC2 read-only policy to Developer group.
```

This gives all users in the Developer group read-only access to EC2.

---

## 20. Resource-Based Policy

A resource-based policy is attached directly to a resource.

Example resources that support resource-based policies:

* S3 buckets
* Lambda functions
* SQS queues
* SNS topics
* KMS keys

Example:

```text
S3 bucket policy allows another AWS account to read objects.
```

---

## 21. AWS Managed Policy

An **AWS managed policy** is created and managed by AWS.

Example:

```text
AmazonS3ReadOnlyAccess
AmazonEC2ReadOnlyAccess
AdministratorAccess
CloudWatchReadOnlyAccess
```

Advantages:

* Easy to use
* Maintained by AWS
* Suitable for common permission sets

Disadvantage:

* May be broader than needed

---

## 22. Customer Managed Policy

A **customer managed policy** is created and managed by you.

Example:

```text
Allow developers to start and stop only selected EC2 instances.
```

Advantages:

* Custom permissions
* Better control
* Can follow least privilege more closely

---

## 23. Inline Policy

An **inline policy** is directly attached to one IAM user, group, or role.

It is not reusable.

Example:

```text
Inline policy attached only to user John.
```

Managed policies are usually better for reusable permission management.

---

## 24. Permissions Boundary

A **permissions boundary** sets the maximum permissions an IAM user or role can have.

Even if a policy grants more permissions, the permissions boundary limits the final permission.

Simple example:

```text
Policy allows: S3 and EC2
Permissions boundary allows: S3 only

Final allowed permission: S3 only
```

Permissions boundaries are useful in large organizations where teams create their own IAM roles but must stay within limits.

---

## 25. Service Control Policy — SCP

A **Service Control Policy** is used with AWS Organizations.

SCPs define the maximum permissions available to accounts in an organization.

Example:

```text
Deny all users from creating resources outside Australia region.
```

Important:

```text
SCP does not grant permissions.
SCP only sets permission limits.
```

---

## 26. Session Policy

A session policy is used when temporary credentials are created.

It limits the permissions for that temporary session.

Example:

```text
User assumes a role.
Session policy further restricts what the role session can do.
```

---

## 27. How IAM Decides Access

When a user or role makes a request, AWS evaluates permissions.

Basic permission logic:

```text
1. By default, everything is denied.
2. If there is an Allow policy, the action may be allowed.
3. If there is an explicit Deny, the action is denied.
4. Explicit Deny overrides Allow.
```

Simple example:

```text
Policy A: Allow s3:GetObject
Policy B: Deny s3:GetObject

Final result: Denied
```

---

## 28. Least Privilege Principle

The principle of **least privilege** means giving only the permissions required to perform a task.

Bad example:

```json
{
  "Effect": "Allow",
  "Action": "*",
  "Resource": "*"
}
```

This gives full access to everything.

Better example:

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:ListBucket"
  ],
  "Resource": [
    "arn:aws:s3:::my-training-bucket",
    "arn:aws:s3:::my-training-bucket/*"
  ]
}
```

This gives only the required S3 read permissions.

---

## 29. Multi-Factor Authentication — MFA

**MFA** adds an extra security layer to AWS login.

Without MFA:

```text
Username + Password
```

With MFA:

```text
Username + Password + MFA code
```

MFA can use:

* Authenticator app
* Hardware security key
* Virtual MFA device

Best practice:

```text
Enable MFA for root user.
Enable MFA for IAM users.
Require MFA for sensitive actions.
```

---

## 30. Access Keys

Access keys are used for programmatic access to AWS.

They are commonly used with:

* AWS CLI
* AWS SDK
* Applications
* Scripts

An access key has:

```text
Access key ID
Secret access key
```

Example AWS CLI configuration:

```bash
aws configure
```

It asks for:

```text
AWS Access Key ID
AWS Secret Access Key
Default region
Output format
```

Important security advice:

* Do not share access keys.
* Do not upload access keys to GitHub.
* Rotate access keys regularly.
* Delete unused access keys.
* Prefer IAM roles instead of long-term access keys where possible.

---

## 31. IAM Role for EC2

When an EC2 instance needs to access another AWS service, use an IAM role.

Example:

```text
EC2 instance needs to read files from S3.
```

Bad approach:

```text
Store access keys inside EC2 instance.
```

Better approach:

```text
Create IAM role with S3 read permission.
Attach role to EC2 instance.
EC2 receives temporary credentials automatically.
```

Example architecture:

```text
EC2 Instance
   ↓ assumes
IAM Role
   ↓ allows
S3 Read Access
```

---

## 32. Example IAM Policy: S3 Read-Only Access to One Bucket

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadOnlyAccessToSpecificBucket",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::my-training-bucket",
        "arn:aws:s3:::my-training-bucket/*"
      ]
    }
  ]
}
```

Explanation:

| Policy Part | Meaning                            |
| ----------- | ---------------------------------- |
| Effect      | Allows access                      |
| Action      | Allows listing and reading objects |
| Resource    | Applies only to one bucket         |
| Sid         | Optional statement identifier      |

---

## 33. Example IAM Policy: Start and Stop EC2 Instances

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "StartStopEC2",
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

This policy allows a user to start and stop EC2 instances.

---

## 34. Example IAM Policy: Deny Access Without MFA

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyIfMFAIsNotEnabled",
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"
        }
      }
    }
  ]
}
```

This policy denies actions when MFA is not present.

---

## 35. IAM and AWS CLI

IAM permissions also control what users can do through AWS CLI.

Example command:

```bash
aws s3 ls
```

If the user has permission, the command works.

If the user does not have permission, AWS returns an error such as:

```text
AccessDenied
```

Example:

```text
User tries: aws ec2 describe-instances
IAM checks: Does this user have ec2:DescribeInstances permission?
```

---

## 36. IAM and EC2 Security Group Difference

IAM and security groups are different.

| Feature  | IAM                          | Security Group           |
| -------- | ---------------------------- | ------------------------ |
| Purpose  | Controls AWS API permissions | Controls network traffic |
| Used for | Users, roles, services       | EC2, RDS, ENI            |
| Example  | Allow user to start EC2      | Allow HTTP port 80       |
| Works at | AWS permission level         | Network/firewall level   |

Example:

```text
IAM permission:
User can create an EC2 instance.

Security group rule:
Users can access the EC2 website through port 80.
```

Both are important, but they protect different things.

---

## 37. IAM Identity Center

IAM Identity Center is used to manage workforce access to AWS accounts and applications.

It is useful when an organization has many users and AWS accounts.

Example:

```text
Company employees log in through a central identity system.
Then they access assigned AWS accounts and roles.
```

IAM Identity Center is commonly used for:

* Single sign-on
* Multi-account access
* Workforce identity management
* Permission sets

---

## 38. IAM Access Analyzer

IAM Access Analyzer helps identify resources that are shared externally.

Example:

```text
An S3 bucket is accidentally shared with another AWS account.
IAM Access Analyzer can help detect this.
```

It is useful for reviewing access and improving security.

---

## 39. Credential Report

An IAM credential report gives information about IAM users and their credentials.

It can show:

* Users
* Password status
* MFA status
* Access key status
* Last password use
* Last access key use

It is useful for security auditing.

---

## 40. Common IAM Best Practices

Important IAM best practices:

* Do not use the root user for daily tasks.
* Enable MFA for the root user.
* Enable MFA for IAM users.
* Follow least privilege.
* Use IAM roles instead of long-term access keys where possible.
* Rotate access keys regularly.
* Remove unused users, roles, and keys.
* Use groups to manage user permissions.
* Avoid giving `AdministratorAccess` unless required.
* Use customer managed policies for better control.
* Review permissions regularly.
* Use IAM Access Analyzer.
* Use CloudTrail to monitor AWS API activity.
* Do not store access keys in code.
* Do not upload credentials to GitHub.
* Use temporary credentials for applications and services.

---

## 41. Common IAM Mistakes

| Mistake                              | Why It Is a Problem                     |
| ------------------------------------ | --------------------------------------- |
| Using root user for daily work       | Very risky because root has full access |
| Not enabling MFA                     | Account is less secure                  |
| Giving full admin access to everyone | Violates least privilege                |
| Sharing access keys                  | Can lead to account compromise          |
| Uploading keys to GitHub             | Attackers can misuse the account        |
| Using one IAM user for many people   | No accountability                       |
| Not deleting unused users            | Security risk                           |
| Not rotating access keys             | Long-term exposure risk                 |
| Using `Action: "*"` unnecessarily    | Gives too much access                   |
| Using `Resource: "*"` unnecessarily  | Gives broad resource access             |

---

## 42. Simple IAM Example

Scenario:

```text
A developer needs to view EC2 instances but should not stop or terminate them.
```

Correct approach:

```text
Create IAM group: EC2ReadOnlyGroup
Attach policy: AmazonEC2ReadOnlyAccess
Add developer user to the group
Enable MFA for the user
```

Result:

```text
Developer can view EC2 instances.
Developer cannot stop or delete EC2 instances.
```

---

## 43. IAM Example with EC2 and S3

Scenario:

```text
An application running on EC2 needs to read files from S3.
```

Correct approach:

```text
1. Create IAM role.
2. Attach S3 read-only policy to the role.
3. Attach role to EC2 instance.
4. Application accesses S3 using temporary role credentials.
```

Architecture:

```text
EC2 Application
      ↓
IAM Role
      ↓
S3 Bucket
```

This avoids storing access keys inside the EC2 server.

---

## 44. IAM Policy Evaluation Summary

IAM permission checking can be understood like this:

```text
Request made by user/role
        ↓
AWS checks identity policies
        ↓
AWS checks resource policies
        ↓
AWS checks permissions boundaries
        ↓
AWS checks SCPs if using AWS Organizations
        ↓
AWS checks explicit Deny
        ↓
Final decision: Allow or Deny
```

Important rule:

```text
Explicit Deny always wins.
```

---

## 45. IAM Summary

IAM is one of the most important security services in AWS.

Key points to remember:

```text
IAM = Identity and Access Management

User = Person or application with AWS access
Group = Collection of users
Role = Temporary permission identity
Policy = JSON document defining permissions
Permission = What actions are allowed or denied
MFA = Extra login security
Access key = Programmatic access credential
Least privilege = Give only required permissions
```

---

## 46. Final Simple Explanation

In simple words:

> IAM is the security control system of AWS. It controls who can log in, what they can access, and what actions they can perform.

Example:

```text
User logs in to AWS
      ↓
IAM authenticates the user
      ↓
User tries to open S3
      ↓
IAM checks permissions
      ↓
Access is allowed or denied
```

IAM protects AWS resources by making sure users and services only have the permissions they actually need.
