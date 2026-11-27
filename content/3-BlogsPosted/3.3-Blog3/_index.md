---
title: "Blog 3"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Exploring AWS Amplify: My Perspective on Architecture and Practical Considerations

## 1. What Exactly Is AWS Amplify?

Simply put, AWS Amplify is not a separate infrastructure independent of AWS. It is an orchestration layer built on top of familiar core services such as Amazon S3, Amazon CloudFront, Amazon Cognito, Amazon DynamoDB, and AWS Lambda.

Instead of manually configuring each service and establishing IAM permissions between services, Amplify combines these processes into a unified platform, allowing frontend and mobile developers to deploy their products more quickly.

### Differences Between AWS Amplify Gen 1 and Gen 2

While exploring AWS documentation, I learned that AWS Amplify has two generations: Gen 1 and Gen 2. The main difference lies in their approaches to application development and deployment.

#### Amplify Gen 1

- Primarily uses interactive commands through the Amplify CLI or the Amplify Studio interface.
- Automatically generates CloudFormation templates behind the scenes to deploy AWS resources.

#### Amplify Gen 2

- Adopts a Code-first development approach based on AWS CDK.
- All backend components, such as Auth, Data, Storage, and Functions, are defined directly using TypeScript.
- Each team member can use a separate Cloud Sandbox environment for testing without affecting each other's data.

## 2. AWS Amplify Architecture and Workflow

Looking at the architecture diagram, the operation of AWS Amplify can be divided into two main workflows: the developer's application deployment workflow and the user's application access workflow.

### Application Deployment Workflow

1. Developers write code in Visual Studio Code and perform a `git push` to GitHub.
2. GitHub sends a notification (Webhook) to AWS Amplify.
3. AWS Amplify automatically retrieves the latest source code, installs dependencies using `npm ci`, builds the application using `npm run build`, and deploys it to the production environment with HTTPS enabled.

One feature I find particularly useful is **Atomic Deploy**. If a new build encounters an error during deployment, the previously deployed version of the website remains operational, helping minimize system interruptions.

### User Application Access Workflow

When users access the website, the system consists of two main components: Amplify Hosting and Amplify Backend.

#### Amplify Hosting

Amplify Hosting is responsible for deploying and distributing the website's frontend through a global CDN.

The service supports applications such as:

- Single Page Application (SPA): React, Vue.
- Server-Side Rendering (SSR): Next.js, Nuxt.

#### Amplify Backend

Amplify Backend supports four fundamental groups of application functionalities.

| Component | Functionality | AWS Service |
|---|---|---|
| Authentication | Registration, login, authorization, and JWT token issuance. | Amazon Cognito |
| Storage | Storing images, PDF files, and documents. | Amazon S3 |
| Database | Storing application data with automatic scalability. | Amazon DynamoDB |
| Function | Executing server-side logic in response to events. | AWS Lambda |

## 3. Common Misconceptions and Practical Considerations

While learning about AWS Amplify, I noticed several points that can easily cause confusion for those who are new to this service.

### Using Amplify Hosting Does Not Require Amplify Backend

Amplify Hosting and Amplify Backend can be used independently.

Developers can use Amplify Hosting exclusively to deploy the application frontend, similar to Vercel or Netlify.

Meanwhile, the backend can connect to existing AWS services such as:

- Amazon Cognito.
- Amazon API Gateway.
- AWS Lambda.
- Amazon DynamoDB.
- Amazon S3.

Additionally, the application can also connect to a backend deployed outside AWS.

### 404 Errors When Refreshing Pages in SPA Applications

After deploying an SPA application such as React or Vue, the homepage may work normally.

However, when users access a subpage, such as `/books/123`, and refresh the page by pressing F5, the system may return a `404 Not Found` error.

The reason is that the server cannot find the physical file corresponding to the requested path.

To resolve this issue, the following steps can be performed:

1. Access the AWS Amplify Console.
2. Select **Hosting**.
3. Navigate to **Rewrites and redirects**.
4. Add a `200 (Rewrite)` rule to rewrite appropriate SPA paths to `/index.html`.

### Be Careful with Frontend Environment Variables

The AWS Amplify Console provides an **Environment variables** section, allowing developers to easily configure information such as API URLs or Amazon Cognito IDs.

However, environment variables beginning with `VITE_` or `NEXT_PUBLIC_` may be bundled into the source code delivered to the user's browser.

Therefore, sensitive information must never be stored in these variables, such as:

- AWS Access Key.
- AWS Secret Access Key.

This is an important security consideration during application development and deployment.

### When Should You Avoid Using AWS Amplify?

AWS Amplify helps simplify the application deployment process. However, in some cases, manually configuring individual AWS services may provide better control and customization.

Some examples include:

- Systems requiring extensive internal network customization with complex VPC architectures.
- Systems using Microservices architecture on Kubernetes (Amazon EKS).
- Systems requiring detailed customization of Cache policies on Amazon CloudFront or AWS WAF configurations.

In these cases, manually integrating individual AWS services provides developers with greater control over infrastructure design and management.

## 4. Conclusion

AWS Amplify does not replace the need to understand and master core AWS services, but it significantly reduces the time required to transform source code into a fully operational product on the Cloud.

By exploring the workflow from `git push` to Amplify Hosting and how it connects to the underlying backend components, I gained a better understanding of the application deployment process on AWS.

Understanding the operational architecture of AWS Amplify not only makes application deployment faster but also supports the scaling or separation of infrastructure components as the project grows in the future.

### Images

![overview](/images/3-Blog/Blog3/Blog1.jpg)

### Blog Post Link

https://www.facebook.com/groups/660548818043427/my_posted_content
