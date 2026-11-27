
---
title: "Blog 1"
date: 2026-11-27
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Exploring Amazon S3 and Its Application in Storing Book Documents and Images in the Cloud Library Management System

## 1. Overview of Amazon S3

Amazon S3 (Amazon Simple Storage Service) is an object storage service provided by AWS. This service allows users to store and retrieve various types of data, such as images, PDF documents, videos, backup files, and other content.

Unlike databases designed to manage structured information, Amazon S3 focuses on storing and managing data objects.

### Basic Concepts of Amazon S3

- **Bucket:** A container for storing objects. When using Amazon S3, we need to create a bucket before uploading files.

- **Object:** Data stored in a bucket, such as a PDF file or a book cover image. Each object consists of data content and related information.

- **Object Key:** The identifier of an object within a bucket.

For example, in a library system, files can be organized using Object Keys such as:

    books/covers/book001.jpg
    books/pdfs/book001.pdf
    authors/submissions/book002.pdf

This is an example of how data can be organized, not a confirmed storage structure in the project.

- **Storage Class:** Amazon S3 provides various storage classes to meet different access requirements and optimize costs. The selection of a Storage Class should be based on usage frequency and data retrieval requirements.

## 2. File Storage Requirements in Cloud Library

Cloud Library Management System is a group project designed to build a library management system and support users in reading books online.

In this system, data can be divided into two main categories.

### Book Information Data

Includes:

- Book ID.
- Book title.
- Author.
- Category.
- Description.
- Other business-related information.

This type of data can be managed using Amazon DynamoDB.

### File-Based Data

Includes:

- Book cover images.
- PDF documents.
- Files uploaded during system usage.

For the second category, Amazon S3 is a storage solution worth exploring because it is designed to manage objects rather than business data records.

Separating book information from file content also helps establish clearer responsibilities among different system components.

## 3. Combining Amazon S3 and Amazon DynamoDB

A suitable design approach for Cloud Library is to use Amazon DynamoDB to store business information and Amazon S3 to store book-related files.

For example, when a new book is added to the system:

1. The book cover image or PDF file is uploaded to an appropriate storage location.
2. The system records the Object Key or reference information for the file.
3. The book information is stored in DynamoDB.
4. When users open the book details page, the application retrieves the book information and uses the file reference to display the appropriate content.

With this approach, DynamoDB does not need to directly store the entire binary content of PDF files or book cover images.

During the development of Cloud Library, I had the opportunity to examine book data stored in DynamoDB tables and observe how the user interface displays the book catalog.

From this experience, I further explored the role of an object storage service such as Amazon S3 in managing file-based data components.

However, the actual file storage implementation still needs to be verified against the project's configuration and source code.

## 4. Managing PDF Files and Book Cover Images

For an online library application, book cover images and reading materials have different access requirements.

### Book Cover Images

Book cover images are commonly used when displaying the book catalog or book details pages.

If stored in Amazon S3, the application can reference the corresponding objects to retrieve and display images.

For content that is permitted to be publicly accessible, Amazon CloudFront can also be explored to distribute content efficiently while maintaining control over access to Amazon S3.

### PDF Files

Unlike book cover images, some PDF documents may require users to log in or meet specific access conditions before reading.

Therefore, when exploring document storage on Amazon S3, I need to pay attention to access permissions rather than allowing all users to download files directly by default.

### Files Submitted by Authors

Cloud Library provides a feature that allows authors to submit books for approval.

Files awaiting approval need to be managed separately to prevent documents from being publicly accessible before they are approved.

One possible design approach is to store these documents in a private storage area and grant access only to authorized components responsible for processing them.

## 5. Data Security on Amazon S3

While learning about Amazon S3, I realized that access management is a particularly important consideration.

### Block Public Access

Amazon S3 provides the Block Public Access mechanism to prevent unintended public access permissions.

For a library system, especially for documents that are not intended to be freely accessible, maintaining appropriate security configurations is essential.

### IAM Policies and Bucket Policies

These policies help control which identities or components are permitted to perform operations such as uploading, reading, or deleting objects.

### Presigned URL

A Presigned URL is a method of granting temporary access to a specific object without making the entire bucket publicly accessible.

For example, after the system verifies that a user has permission to read a document, the backend can generate a Presigned URL that allows the user to access the file for a specified period.

However, Presigned URLs must be protected because anyone who possesses a valid URL can use the access permissions granted by that URL.

### Data Encryption

Amazon S3 automatically encrypts new objects when they are stored. In addition to the default encryption mechanism, AWS also supports other encryption options to meet the security requirements of different systems.

Through these concepts, I realized that successfully uploading files is only the first step when using Amazon S3. Developers also need to consider how users access files and the scope of the permissions granted.

## 6. Cost and Data Organization Considerations

In addition to security, cost is another factor that needs to be considered when using Amazon S3.

Costs may depend on storage capacity, the number of requests, storage classes, and data transfer activities.

For Cloud Library, the number of book cover images and PDF documents may increase as new books are added to the library. Therefore, proper data organization and management plans are necessary.

Some key points I learned while exploring Amazon S3 include:

- Use clear naming conventions for Object Keys.
- Distinguish between publicly accessible files and files that require authentication.
- Validate file types and file sizes when users upload files.
- Do not grant public access to an entire bucket simply to display certain documents.
- Select appropriate Storage Classes based on access frequency.
- Monitor storage capacity and requests that generate costs.
- Consider data lifecycle policies when the system contains many files that are no longer in use.

## 7. Conclusion

Amazon S3 is an object storage service that can meet various file management requirements in web applications, particularly for systems containing images and documents, such as Cloud Library Management System.

Through learning about Amazon S3, I gained a better understanding of the differences between storing business data in Amazon DynamoDB and storing files as objects.

In addition, I realized that storage design should be considered together with access permissions, data security, and usage costs.

In the future, I would like to continue exploring how to integrate Amazon S3 into the processes of uploading documents, displaying book cover images, and granting access permissions for reading PDF files in the library system.

I hope this article provides a useful perspective for those who are learning about Amazon S3 and how to combine object storage services with other components in real-world applications.

### Images

![overview](/images/3-Blog/Blog1/Blog1.jpg)
![overview](/images/3-Blog/Blog1/Blog2.jpg)

### Blog Post Link

https://www.facebook.com/groups/awsstudygroupfcj/permalink/2350500839048208/?rdid=g55e6bkJsxOjuhG5#
