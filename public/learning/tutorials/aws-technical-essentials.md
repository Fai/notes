---
title: "AWS Technical Essentials"
tags: [learning, tutorial, aws]
created: 2026-02-26
---

AWS provides cloud computing services.
- [What Is Cloud Computing?](https://aws.amazon.com/what-is-cloud-computing/)
- [Types of Cloud Computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/types-of-cloud-computing.html)
- [Cloud Computing with AWS](https://aws.amazon.com/what-is-aws/)
- [Overview of Amazon Web Services](https://docs.aws.amazon.com/pdfs/whitepapers/latest/aws-overview/aws-overview.pdf)

Six advantages of cloud computing
- Pay-as-you-go
- Benefit from massive economies of scale
- Stop guessing capacity
- Increase speed and agility
- Realize cost savings
- Go global in minutes

AWS Global Infrastructure
- **Data Center** operate in discrete facilities in undisclosed locations and connected using redundant high-speed and low-latency links.
- **Availability Zone** consists of one or more data centers with redundant power, networking, and connectivity.
- **Region** are geographic locations worldwide where AWS hosts its data centers. AWS Regions are named after the location where they reside. Inside every Region is a cluster of Availability Zones.
- **Edge locations** are global locations where content is cached.

Choosing the right AWS Region 
- **Data Compliance:** comply with regulations that require customer data to be stored in a specific geographic territory
- **Latency:** choose a Region that is close to your user base
- **Pricing:** prices vary from one Region to another
- **Service Availability:** services might not be available in some Regions

A well-known best practice for cloud architecture is to use Region-scoped, managed services.
When that is not possible, make sure your workload is replicated across multiple Availability Zones. At a minimum, you should use two Availability Zones.

Amazon CloudFront delivers your content through a worldwide network of edge locations. When a user requests content that is being served with CloudFront, the request is routed to the location that provides the lowest latency.

Resource
- [Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
- [AWS Global Infrastructure Documentation](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/global-infrastructure.html)
- [AWS Regions and Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)
- [AWS Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)
- [AWS Regional Services](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)
- [Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

**Interact with AWS**
- AWS Management Console
- AWS Command Line Interface (CLI)
- AWS SDK

Resource
- [What Is the AWS Management Console?](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/)
- [AWS Command Line Interface](https://aws.amazon.com/cli/)
- [Tools to Build on AWS](https://aws.amazon.com/developer/tools/)

**Security and the AWS Shared Responsibility Model**
[Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
AWS is responsible for security of the cloud
- Protecting and securing AWS Regions, Availability Zones, and data centers, down to the physical security of the buildings
- Managing the hardware, software, and networking components that run AWS services, such as the physical servers, host operating systems, virtualization layers, and AWS networking components
You are responsible for security in the cloud
- Properly configuring the service and their applications, in addition to ensuring that their data is secure

**Protecting the AWS Root User**
AWS root user credentials consist of two parts:
- **Access key ID****
- **Secret access key**
Access keys should be managed with the same security as an email address and password.

**AWS root user best practices**
The root user has complete access to all AWS services and resources in your account, including your billing and personal information. Therefore, you should securely lock away the credentials associated with the root user and not use the root user for everyday tasks. Visit the links at the end of this lesson to learn more about when to use the AWS root user.

To ensure the safety of the root user, follow these best practices:
- Choose a strong password for the root user.
- Enable multi-factor authentication (MFA) for the root user.
- Never share your root user password or access keys with anyone.
- Disable or delete the access keys associated with the root user.
- Create an Identity and Access Management (IAM) user for administrative tasks or everyday tasks.

**Multi-factor authentication**
- MFA requires two or more authentication methods to verify an identity.
- AWS supports a variety of MFA mechanisms, such as virtual MFA devices, hardware time-based one-time password (TOTP) tokens, and FIDO security keys.

Resource
- [Enabling a Hardware TOTP token (Console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_physical.html)
- [Enabling a FIDO Security Key (Console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_fido.html)
- [Enabling a Virtual Multi-Factor Authentication (MFA) Device (Console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html)
- [Multi-Factor Authentication for IAM](https://aws.amazon.com/iam/features/mfa/)
- [Tasks That Require Root User Credentials](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)

**AWS Identity and Access Management**

