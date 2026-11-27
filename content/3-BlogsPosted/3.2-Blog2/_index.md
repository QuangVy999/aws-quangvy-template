---
title: "Blog 2"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Exploring Amazon Cognito and Its Application in User Authentication in the Cloud Library Management System Project

## 1. Overview of Amazon Cognito

Amazon Cognito is an AWS service that supports identity management and user authentication for web and mobile applications.

Instead of building an entire authentication system from scratch, developers can use Amazon Cognito to implement features such as account registration, login, contact information verification, and password recovery.

### Basic Components of Amazon Cognito

- **User Pools:** Manage user accounts and perform authentication.

- **Identity Pools:** Provide temporary AWS credentials that allow applications to access AWS resources based on configured permissions.

Within the scope of the Cloud Library project, I focused on learning about and using Amazon Cognito User Pools.

## 2. User Pool and App Client

When configuring Amazon Cognito, the first two components I needed to understand were User Pool and App Client.

### User Pool

A User Pool is a directory that manages the accounts of application users.

Depending on the configuration, users can register with their email addresses, verify their accounts, and log in using permitted authentication methods.

### App Client

An App Client is a configuration for an application that connects to a User Pool.

The App Client defines the authentication methods and related settings that the application can use.

In Cloud Library, I created a User Pool and an App Client in the AWS Asia Pacific (Singapore) Region, then used this configuration to test the system's user account functionalities.

Through this step, I gained a better understanding of how applications connect to a managed authentication service on AWS.

## 3. Applying Amazon Cognito to Cloud Library

Cloud Library Management System is an online library system with multiple user groups, including:

- Readers.
- Authors.
- Librarians.
- Administrators.

Each user group has its own functionalities. Therefore, in addition to managing account information, the system also needs an authentication mechanism to identify the users accessing it.

During the development process, I practiced and tested the following basic functionalities.

### Account Registration

Users enter the required information on the Cloud Library registration interface.

The application sends a registration request to Amazon Cognito to create an account in the User Pool.

### Email Verification

After registration, users need to complete the email verification process according to the system configuration.

I tested the verification code input interface and the account verification process before logging in.

### Login

After the account has been confirmed, users can log in to Cloud Library.

When the login credentials are valid, Amazon Cognito supports authentication and returns the corresponding tokens based on the configured authentication flow.

The application can use the authentication results to identify user sessions and protect functionalities that require users to log in.

### Integration with Application Data

In Cloud Library, Amazon Cognito handles authentication, while Amazon DynamoDB is used to store business data.

For example, when a user adds a book to their favorites list, the system needs to associate that action with the correct user account.

During testing, I checked the storage of the favorites list and confirmed that the data remained available after logging out and logging back in.

Through this experience, I gained a better understanding of the separate roles of an authentication service and a database within the same application.

## 4. Distinguishing Between Authentication and Authorization

One important distinction I recognized when building a system with multiple account types is the difference between Authentication and Authorization.

### Authentication

Authentication is the process of verifying a user's identity, such as through an email address and password.

### Authorization

Authorization is the process of determining which actions an authenticated user is permitted to perform within the system.

For example, in Cloud Library:

| User Group | Functionality |
|---|---|
| Reader | View books and save books to the favorites list. |
| Author | Submit books for approval. |
| Librarian | Perform library management operations. |
| Administrator | Perform system administration functions. |

Amazon Cognito supports authentication and provides identity information.

However, business-level authorization rules still need to be properly enforced by the application and backend, rather than relying solely on hiding or displaying functionalities on the user interface.

This is also an area that I need to continue exploring when the system has multiple user groups with different access permissions.

## 5. Challenges and Lessons Learned

When I first started learning about Amazon Cognito, I needed time to understand the differences between components such as User Pool, App Client, and the various steps in the account verification process.

In addition, connecting the registration and login interfaces to AWS required careful verification of configuration parameters and how the application handled the returned results.

Through practical experience, I identified several important considerations:

- Clearly define registration and login methods from the beginning.
- Thoroughly test the entire process of registration, email verification, login, and logout.
- Do not store user passwords in business data tables.
- Distinguish between account authentication and access permission verification.
- Do not expose sensitive information such as passwords, tokens, or App Client Secrets in publicly accessible source code.
- When developing functionalities that require users to log in, the backend needs to validate tokens and verify permissions to perform the requested actions.

An important lesson for me is that account-related functionalities need to be tested throughout the entire workflow, rather than simply checking whether the login process is successful.

## 6. Conclusion

Through learning about and practicing with Amazon Cognito, I realized that using a managed authentication service helps reduce the amount of work required to build and operate an authentication system independently.

For the Cloud Library Management System project, Amazon Cognito is an important component in the development of user account functionalities.

In addition to learning how to configure User Pool and App Client, I also gained a better understanding of the differences between authentication, authorization, and business data management.

In the future, I would like to further explore token mechanisms, access permission verification, and API protection when integrating Amazon Cognito into real-world applications.

I hope this article provides a useful perspective for those who are new to Amazon Cognito and want to understand how to apply this service to a web application project.

### Images

![overview](/images/3-Blog/Blog2/Blog1.jpg)
![overview](/images/3-Blog/Blog2/Blog2.jpg)
![overview](/images/3-Blog/Blog2/Blog3.jpg)
![overview](/images/3-Blog/Blog2/Blog4.jpg)

### Blog Post Link

https://www.facebook.com/groups/awsstudygroupfcj/permalink/2349109312520694/?rdid=ZYnH7TNOLWm64Cin#
