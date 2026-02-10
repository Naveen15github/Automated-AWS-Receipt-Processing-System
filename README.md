# Automated-AWS-Receipt-Processing-System

![Alt text](image_url "Optional title")

## Overview

This project is an **Automated AWS Receipt Processing System** that streamlines the handling of receipts by automatically extracting key information and storing it in a structured database. Traditionally, processing receipts manually is **time-consuming, error-prone, and difficult to scale**. This system uses **AWS serverless services** to perform OCR, store the extracted data, and notify users, all in a scalable, real-time workflow.

The goal is to **replace manual data entry** with a fully automated, reliable, and cost-effective solution, while keeping the system within the **AWS Free Tier**.

---

## Architecture 🏗️

The architecture of this system is **modular**, with each AWS service performing a dedicated role:

1. **Storage Layer (Amazon S3)**
   Receipts (images or PDFs) are uploaded to an S3 bucket. The bucket acts as the **entry point** for the workflow and also stores the files for auditing purposes.

2. **Processing Layer (AWS Lambda + Amazon Textract)**
   A Lambda function is triggered whenever a new receipt is uploaded. Lambda calls **Textract**, which performs **AI-powered OCR** to extract text and structured data like `Date`, `Vendor`, and `Amount`.

3. **Database Layer (Amazon DynamoDB)**
   The extracted data is stored in a **NoSQL DynamoDB table**, making it easy to query receipts by attributes like date, vendor, or total amount.

4. **Notification System (Amazon SES)**
   After processing, an email notification is sent using **Amazon Simple Email Service (SES)** containing receipt details.

5. **Security Layer (IAM Roles & Policies)**
   All services communicate securely through **custom IAM roles**, ensuring that each service has **least-privilege access** to perform its tasks.

![Alt text](image_url "Optional title")

---

## AWS Services Used 🛠️

| Service                  | Purpose                                                         |
| ------------------------ | --------------------------------------------------------------- |
| **Amazon S3**            | Stores uploaded receipt images/PDFs securely.                   |
| **AWS Textract**         | Extracts text and structured data from receipts.                |
| **Amazon DynamoDB**      | Stores extracted receipt data in structured format.             |
| **AWS Lambda**           | Automates the processing workflow in real-time.                 |
| **Amazon SES**           | Sends email notifications containing extracted receipt details. |
| **IAM Roles & Policies** | Provides secure access between services.                        |

---

## Step-by-Step Implementation 🔧

### 1. Storage & Database Setup

1. **S3 Bucket**

![Alt text](image_url "Optional title")

   * Create an S3 bucket (e.g., `receipt-storage`) for uploading receipts.
   * Enable versioning and server-side encryption for security.
   * Configure event notifications to trigger Lambda on `PUT` events.

3. **DynamoDB Table**

![Alt text](image_url "Optional title")

   * Create a DynamoDB table (e.g., `Receipts`).
   * Use `ReceiptID` as the primary key.
   * Add attributes for `VendorName`, `Date`, `TotalAmount`, and `S3FilePath`.

---

### 2. Notification Setup

![Alt text](image_url "Optional title")

![Alt text](image_url "Optional title")


1. **Amazon SES Setup**

   * Verify the sender email address.
   * Configure a test recipient email for notifications.
   * SES sends an email every time a receipt is processed successfully.

---

### 3. Processing Setup (Lambda Function)

![Alt text](image_url "Optional title")

![Alt text](image_url "Optional title")


1. **Lambda Function Creation**

   * Runtime: Python 3.9+
   * Trigger: S3 `PUT` event
   * Role: Attach a custom IAM role with permissions to S3, Textract, DynamoDB, and SES.

2. **Python Workflow in Lambda**

   * Fetch the uploaded receipt from S3.
   * Call **Textract AnalyzeDocument API** to extract structured data.
   * Map extracted fields (Vendor, Date, Amount) to DynamoDB attributes.
   * Insert a new item in DynamoDB.
   * Send email notification using SES with extracted data.

---

### 4. Integration & Testing

![Alt text](image_url "Optional title")

1. Upload a sample receipt to the S3 bucket.
2. Lambda is automatically triggered.
3. Data is extracted via Textract and stored in DynamoDB.
4. SES sends an email with receipt details.
5. Check **CloudWatch Logs** for Lambda execution details.

---

## Clean-Up Procedure 🗑️

To avoid unnecessary costs:

1. **Delete S3 Bucket:** Remove all uploaded files first.
2. **Stop Textract Processing:** No further API calls should be made.
3. **Delete DynamoDB Table:** Clear stored data before deletion.
4. **Disable SES Notifications:** Remove verified email addresses.
5. **Remove IAM Roles & Policies:** Delete Lambda execution role.

---

## Key Takeaways

* Fully **serverless architecture** with real-time processing.
* Automated **receipt extraction, storage, and notification**.
* Scalable and cost-efficient using AWS Free Tier.
* Modular setup allows easy integration of additional features like analytics or reporting.
