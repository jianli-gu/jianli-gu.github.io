---
layout: default
title: IAM
parent: AWS
permalink: notes/aws/iam
last_modified_date: 2020-07-11T22:47:08+0000
---

# IAM
{: .no_toc }

*Identity and Access Management*

With `boto3`, use low-level client representing:

```python
client = boto3.client("iam")
```

or, high-level resource representing:

```python
iam = boto3.resource("iam")
```

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Resources

### Users

IAM user is an entity created in the AWS account and used by a person or applicatoin to interact
with AWS resources. I has permanent long-term credentials and can be used to directly interact
with AWS services.

### Groups
IAM group is a collection of IAM users.

### Roles

IAM role is an IAM identity created with specific permissions. It does not have any credentials
and cannot make direct requests to AWS services. IAM roles can be assumed by authorized entities,
like AWS users, applications, or another AWS service.

### Policies

The policy types are listed below in order of frequency.

* Identity-based policies
* Resource-based policies
* Permission boundaries
* Organization service control policy (SCP)
* Access control lists (ACLs)
* Session policies

## Identifiers

The followings show the syntax of identifiers (aka., ARNs) about different IAM resources.

```bash
arn:aws:iam::account-id:root
arn:aws:iam::account-id:user/user-name-with-path
arn:aws:iam::account-id:group/group-name-with-path
arn:aws:iam::account-id:role/role-name-with-path
arn:aws:iam::account-id:policy/policy-name-with-path
arn:aws:iam::account-id:instance-profile/instance-profile-name-with-path
arn:aws:sts::account-id:federated-user/user-name
arn:aws:sts::account-id:assumed-role/role-name/role-session-name
arn:aws:iam::account-id:mfa/virtual-device-name-with-path
arn:aws:iam::account-id:u2f/u2f-token-id
arn:aws:iam::account-id:server-certificate/certificate-name-with-path
arn:aws:iam::account-id:saml-provider/provider-name
arn:aws:iam::account-id:oidc-provider/provider-name
```


## Best Practices

* Lock away your AWS account root user access keys.
* Create individual IAM users.
* User groups groups to assign permissions to IAM users.
* Grant least privilege.
* Get started using permissions with AWS managed policies.
* User customer managed policies instead of inline policies.
* Use access leveels to review IAM permissions.
* Configure a strong password policy for your users.
* Enable MFA.
* Use roles for applications that run on Amazon EC2 instances.
* Use roles to delegate permissions.
* Do not share access keys.
* rotate credentials regularly.
* Remove unnecessary credentials
* Use policy conditions for extra security.
* Monitor activity in your AWS account.

Please for this [link](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
to review the best practice details.


## References

[1] [https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
