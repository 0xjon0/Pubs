# AWS_Security_Studying.md
A consolidated resource of things I found elsewhere. I take no credit for this content.

Some References:
- https://github.com/jassics/security-interview-questions/blob/main/aws-security-interview-questions.md
- https://www.ziprecruiter.com/career/job-interview-question-answers/aws-security-consultant
- https://aws.plainenglish.io/
- https://www.reddit.com/r/leetcode/comments/1h0ijby/amazon_sde1_interview_experience_rejected/
- https://assets.aboutamazon.com/d4/9b/6d5662ec4a75961ae78c473e7d03/amazon-leadership-principles-070621-us.pdf

## Security fundamental questions
- How do OWASP Top 10 threats manifest in a FedRAMP Moderate/High AWS application environment?
- Which crypto primitives are approved for use in DoD or IC cloud deployments?
- Why is Perfect Forward Secrecy critical in TLS communications across C2S/SC2S or GovCloud regions?
- How does TLS 1.3 impact boundary control configurations in a federal cloud environment?
- How does OAuth differ from SAML in the context of implementing ICAM solutions across multi-cloud federal systems?
- Stream Cipher vs Block Cipher
- Encryption vs Hashing vs Encoding vs Obfuscation
- Why XoR is very important in the Crypto world

## Application Security Basics Questions
- What’s the difference between Static Application Security Testing (SAST) and Software Composition Analysis (SCA)? How would you incorporate both into a CI/CD pipeline hosted on AWS CodePipeline for a high-side app?
- Explain XSS with an example and how it can be avoided. How do you enforce CSP and mitigate XSS in a classified AWS workspace using services like CloudFront and WAF?
- What IAM, WAF, or VPC-level protections would you recommend to prevent brute-force attacks in a zero-trust DoD enclave? How would you review an architecture to prevent automated brute-force attacks?
- Explain CORS, Same-Origin Policy (SOP), and Content Security Policy (CSP) from a security point of view.
- How is Cross-Site Request Forgery dangerous and what must be done to prevent CSRF in an application?
- Describe the various phases of SDL and the security activities involved in each phase.
- What are some strategies for ensuring secure session management in web applications?

## Overall Security Assessment-based Questions
- How do you handle security misconfigurations in development and production environments?
- How can an attacker exploit Server-Side Request Forgery, and how can you prevent SSRF? Include detection and mitigation of SSRF attacks when dealing with legacy applications still using IMDSv1.
- List techniques used to prevent web server attacks
- When assessing an AWS environment for FedRAMP compliance, how do you validate configuration baselines (e.g., EC2, S3)?

## “Test Your Problem-Solving Skills” AppSec Questions (Senior Roles)
- How would you design a security strategy to protect a microservices architecture from both external and internal threats? What are the challenges you might face while designing and implementing it?
- Describe how you would conduct threat modeling for a cloud-native application, and for a CUI-handling workload in AWS GovCloud aligned to NIST 800-53 and FedRAMP.
- Design a secure architecture for a DoD HR platform using AWS services (GovCloud or SC2S), covering data flow, access control, logging, and segmentation.

## Explain How You Handle AuthN and AuthZ
- Describe implementing fine-grained RBAC in AWS using IAM roles, ABAC, and SCPs for a multi-tenant IC customer.
- What are the key differences between using Cognito vs SAML-based federated identity for DoD applications?

## AWS Security Interview Questions
1. How would you investigate GuardDuty or CloudTrail findings indicating lateral movement in a classified VPC?
2. Explain the security differences between CloudTrail and CloudWatch in supporting an ATO package for a FedRAMP system.
3. Detail how IMDSv1 SSRF risks are addressed via mandatory IMDSv2 enforcement in hardened EC2 baselines.
    Pre-2023 AWS used IMDSv1 by default, which is vulnerable to SSRF because it lacks a session authentication and is therefore accessible by any process in the instance.  IMDSv2 requires session-based tokens. Some AMIs/Regions may still default to v1 and existing v1 need to be migrated.
4. Can you explain how to use and when to use Access key id and Principal id with one example?
    Access Key ID is used for programmatic access, while Principal ID uniquely identifies the IAM entity.
5. How would you securely aggregate logs across multiple AWS accounts for IC customers while maintaining compliance?
6. Can you analyse and explain risk and security issues for ElasticSearch services?
7. When should you use AWS Transit Gateway (TGW)? Differentiate from IGW, VGW.
8. Should we expose Database access publicly or to web application directly?
9. Can you help me to understand the security posture for Wordpress site being hosted in AWS?
10. You are trying to SSH into an EC2 instance but it is failing. (Security Groups)
11. What issue you see when any API endpoint is exposed to public?
12. What are the security implications of having multiple open ports in a default security group, and how can you mitigate potential risks?
13. RTO (Recovery Time Objective) vs RPO (Recovery Point Objective)
14. We want to enable SSO and integrate a tool call like Trello in our AWS environment where other applications are hosted. What security posture you think we should work on to keep everything secured?
15. How the security alerts of AWS resources being captured and sent to security people automatically?
16. What are the different data sources for GuardDuty?
17. Walk through the lifecycle of credential rotation for an IAM access key tied to a service account in a DoD system.

## AWS IAM
- An event is triggered and lambda does something. To make sure it works, what you check in IAM?
- How do you enforce MFA across all IAM users and roles in a multi-account AWS environment for federal customers?
- How do you audit and enforce least privilege across hundreds of accounts managed via AWS Organizations?

- Explain below IAM policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "DenyAllUsersNotUsingMFA",
        "Effect": "Deny",
        "NotAction": "iam:*",
        "Resource": "*",
        "Condition": {"BoolIfExists":
                        {"aws:MultiFactorAuthPresent": "false"}
                    }
    }]
}
```
- Explain below policy. What's wrong with this policy?
```json
{
    "Version": "2012-10-17",
    "Statement": {
    "Effect": "Deny",
    "Action": "s3:*",
    "NotResource": [
      "arn:aws:s3:::HRBucket/Payroll",
      "arn:aws:s3:::HRBucket/Payroll/*"
    ]
    }
}
```
- What comes to your mind when a service needs cross account access?

## AWS Data Security
- How do you verify encryption in transit and at rest across all services in a GovCloud landing zone?
- What's your strategy for implementing centralized KMS key management across enclaves with varying classification levels?
- Justify your key rotation policy for symmetric KMS keys supporting a FedRAMP Moderate data workload.


## AWS Scenario Based questions:
### Scenario (Lambda, SES and config rules) :
Task is to create lambda function for config rules and send email using Simple Email Service (SES).
I have multi account and one of the account is for organization level aggregator. Using that org aggregator account write a lambda function to retrieve all non-compliant config rules based on each aggregator. I wanted to have a list which has the following results

Aggregator name = AggregatorTest
OrgConfigRule – VPCFlowlog-o30sfig, NON_COMPLIANT, Acc No: 23135134235
		Account id, Resource id, Resource type, AWS Region
		Account id, Resource id, Resource type, AWS Region

So above we have an email configured for aggregator test and then got the list of all OrgConfigRule then after that whatever resources are there for that org config rule you need to list out and then send email to users using SES.

### Logging, Monitoring & Incident Response
- What are the key components of a robust logging and monitoring strategy for AWS security?
- How would you automate notification of non-compliant AWS Config rules to a centralized security operations email in a multi-account org?
- What is your process for ensuring data integrity of CloudTrail logs under FISMA High or IC compliance needs?
- How do you use EventBridge or Lambda to respond to findings in Security Hub in a low-latency, high-side environment?

### Infrastructure security
- Walk through automating patch management for EC2 instances using SSM Patch Manager in a classified AWS region.
- What findings from AWS Inspector would block a system from receiving an ATO, and how do you remediate?

### Data protection
- KMS key usage: s3 bucket, file download but can’t see the object. what solution do you propose?
- How do you enable automatic rotation for AWS KMS keys, and how can you monitor these rotations using EventBridge?​
    AWS KMS supports automatic rotation of symmetric keys every 365 days.
- What comes to your mind when you have to secure RDS instance?

## Common Aws Security Consultant Interview Questions, Answers
- How do you approach conducting a security assessment for an existing AWS environment?
  - I typically start a security assessment by reviewing the existing AWS architecture, identifying potential vulnerabilities, and evaluating the effectiveness of security controls. This involves examining configurations, permissions, and access controls across services.
- Explain the importance of AWS Well-Architected Framework in the context of security consulting.
  - The AWS Well-Architected Framework provides a set of best practices to ensure that workloads are secure and optimized. I use it to assess the security aspects of an architecture, focusing on areas like identity and access management, data protection, and monitoring.
- What are the key considerations when designing a multi-account strategy for security in AWS?
  - When designing a multi-account strategy for security, I consider the organization's structure and security requirements. I aim to isolate environments based on sensitivity, using AWS Organizations to centrally manage policies and apply consistent security measures. AWS Organizations allows for centralized management and application of service control policies (SCPs).
- How do you ensure secure access management in a complex AWS environment with numerous users and roles?
  - Secure access management in a complex environment involves using AWS IAM effectively. I ensure the principle of least privilege, implement strong authentication mechanisms, and regularly review and audit user and role permissions.
- Discuss the role of AWS Organizations in managing security at scale.
  - AWS Organizations plays a crucial role in managing security at scale. It helps in centralizing policy application, automating account creation, and simplifying billing. This ensures a consistent and secure approach across multiple AWS accounts.
- Explain the process of incident response in AWS, including the use of AWS CloudTrail and AWS Config.
  - Incident response in AWS begins with using AWS CloudTrail and AWS Config to identify and investigate security incidents. I have predefined response plans, and I regularly conduct tabletop exercises to ensure the team is well-prepared.
- What are the key components of a robust logging and monitoring strategy for AWS security?
  - A comprehensive logging and monitoring strategy involves using AWS CloudWatch, CloudTrail, and other services to collect and analyze logs. I set up alerts for suspicious activities, ensuring proactive detection and response to security incidents. CloudTrail is primarily for auditing API calls, while CloudWatch is used for monitoring performance metrics and logs.
- How would you design and implement a robust backup and recovery solution for critical data in AWS?
  - Designing a backup and recovery solution includes regular backups of critical data using services like Amazon S3 or Amazon RDS. I verify the integrity of backups and regularly test the recovery process to ensure its effectiveness.
- How do you assess and mitigate security risks associated with third-party integrations in an AWS environment?
  - Assessing third-party integrations involves reviewing their security documentation, understanding data access requirements, and performing security assessments. I prioritize integrations with strong security postures and work with vendors to address any identified risks.
- How do you perform a gap assessment between an IC customer’s current AWS environment and ICD 503 or CNSSI 1253 compliance?
- How do you educate customer stakeholders on secure AWS practices while balancing delivery velocity?
- Describe your approach to authoring or reviewing a System Security Plan (SSP) as part of FedRAMP or IC accreditation.

## ACE Your AWS Interview (PlainEnglish.io)
- What is CloudFormation and why is it used for?
  - CloudFormation is a service provided by AWS that allows you to create and manage AWS infrastructure as code. AWS CloudFormation templates are written in JSON or YAML, which describe the AWS resources you want to create, configure, and connect to each other. CloudFormation is used to automate the deployment of AWS infrastructure, making it faster and more efficient. It also enables you to version control your infrastructure and apply changes in a controlled and repeatable manner. Additionally, it provides a consistent and standardized way to deploy infrastructure, which reduces the chance of configuration errors and ensures that resources are created in a consistent state.
- Difference between AWS CloudFormation and AWS Elastic Beanstalk?
  - CloudFormation is used for infrastructure management and automation, while Elastic Beanstalk is used for application deployment and management.
  - AWS CloudFormation is an Infrastructure as Code (IaC) service that allows you to define and provision AWS infrastructure and resources using a template. It is used to manage infrastructure resources such as EC2 instances, S3 buckets, and RDS databases.
  - AWS Elastic Beanstalk, on the other hand, is a Platform as a Service (PaaS) that simplifies the deployment and management of web applications. It abstracts away the underlying infrastructure and allows developers to focus on writing code. Elastic Beanstalk automatically provisions and deploys the necessary resources like EC2 instances, load balancers, and databases.
- What are the kinds of security attacks that can occur on the cloud? And how can we minimize them?
  - DDoS attacks, phishing attacks, data breaches, and insider attacks. To minimize them, it is important to use strong authentication methods, implement proper access control and monitoring, encrypt data, keep systems up to date with security patches, and regularly conduct security audits and training for employees.
- Can we recover the EC2 instance when we have lost the key?
  - If you've lost the key pair, you cannot directly access the instance. AWS provides the AWSSupport-ResetAccess runbook via Systems Manager to regain access.
- What is the difference between the Amazon Rds, Dynamodb, and Redshift?
  - Amazon RDS: Relational database service that allows you to set up, operate, and scale a relational database in the cloud easily.
  - DynamoDB: Fully-managed NoSQL database that provides fast and predictable performance with seamless scalability.
  - Redshift: Data warehousing service that provides fast querying and analysis of data using SQL and business intelligence tools.
- Do you prefer to host a website on S3? What’s the reason if your answer is either yes or no?
  - Yes, it can be a good option to host a website on S3. Hosting a website on S3 is a cost-effective solution for static websites that don’t require server-side processing. S3 is a reliable and scalable storage service that offers high availability and durability.
  - However, hosting a dynamic website with server-side processing on S3 is not recommended as it lacks the necessary compute resources and scalability to handle such websites. In such cases, it’s better to use other AWS services like EC2 or Elastic Beanstalk. S3 static website hosting doesn't support HTTPS natively; integration with CloudFront is required for HTTPS.
- What is a load balancer? Give scenarios of each kind of balancer based on your experience.
  - Application Load Balancers (ALBs) distribute traffic based on content at the application layer (HTTP/HTTPS).
  - Network Load Balancers (NLBs) distribute traffic at the transport layer (TCP/UDP).
  - Classic Load Balancers (CLBs) distribute traffic for legacy applications across multiple EC2 instances and can be used with both HTTP and HTTPS protocols.

## Behavioral-based questions
- Tell me about a time you had to persuade a federal stakeholder to abandon an insecure architectural decision.
- Describe a situation where you earned trust from a skeptical security lead by demonstrating AWS technical depth.
- Explain how you handled a failure in a secure cloud deployment and how you led the team through corrective action.
- Tell me about a time you faced a problem with multiple possible solutions. How did you decide what to do, and what was the outcome?
- Describe a time when you took a risk, made a mistake, or failed. What happened, and what did you learn from it?
- Share an example of when you took the lead on a project.
- What did you do to motivate a group or encourage collaboration on a team project?
- How have you used data to develop a strategy or make a decision?
- Tell me about a time when You overcame challenges in your work
- Tell me about a time when You realized you couldn't deliver on something you committed to
- Tell me about a time when You made a bad decision and what would you do differently
- Tell me about a time when You took on an assignment outside of your normal responsibilities
- Tell me about a time when You delivered something that exceeded expectations
- Tell me about a time when You disagreed with someone but was able to move forward with the disagreement
- Tell me about a time when You received harsh critical feedback
- Tell me about the last time you took ownership of something in your job.
- Last time you had to deep dive into a particular bug or task.
- Last time you had a conflict with a co-worker/manager.
- The last time you learnt something that wasn't required at your job, what was your way of learning, and how much time did it take?
- Describe an issue you faced in a production environment in your previous company.
- Give me an example of a time when you asked for customer feedback. How do you used it? (Customer obsession)
- Describe a difficult interaction you had with a customer. How did you deal with it? (Customer obsession)
- What was the most technically challenging project you worked on? Did you face a major issue in production? Did you ever miss your deadlines? Why?
- What was the most innovative challenge project you worked on?  Walk me through a feature you developed. How did you measure its success?
- Tell me about a time when you responded to a security incident. What was your role?
- Describe how you've handled an environment that failed a security audit. What did you do next?
- Have you ever had to explain a complex security issue to a non-technical stakeholder? How did you do it?
- Tell me about a time you made a decision without complete data. How did you proceed?
- Describe a time when you had to rapidly learn a new tool, language, or service to solve a problem.
- Describe a time when you went beyond the scope of your role to help a customer.
- Give an example of a time when a customer’s needs conflicted with best security practices. How did you handle it?
- Tell me about a time when you invented a new way to solve a security or operations problem.
- Have you ever challenged an established process or policy that was no longer effective? What happened?
- Describe a time when you had to align multiple stakeholders with different priorities.
- Tell me about a project where you had no direct authority but had to influence outcomes.
- Tell me about a time you had to prioritize multiple tasks with tight deadlines. How did you decide what to do first?
- Describe a situation where you were given ambiguous instructions or goals. How did you proceed?
- Tell me about a time you helped someone else grow or succeed at work.
