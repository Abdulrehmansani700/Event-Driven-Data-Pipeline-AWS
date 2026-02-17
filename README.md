# Event-Driven Data Pipeline on AWS
This repository contains an implementation of an **Event-Driven Architecture (EDA)** using AWS services. The pipeline automatically processes data uploaded to an S3 bucket and moves it to a target destination after processing.

## 📌 Project Overview
The goal of this project is to demonstrate a decoupled, scalable, and reliable way to handle data events. By using a "Fan-out" pattern with SNS and SQS, we ensure that the system can handle high traffic and multiple consumers without losing data.

### Workflow:
1.  **S3 Bucket (Source):** Acts as the **Event Producer**. When a file (e.g., `stock_pricing.csv`) is uploaded, it triggers an event.
2.  **Amazon SNS:** The **Event Ingestion** layer. It captures the event from S3 and broadcasts it to all subscribers.
3.  **Amazon SQS:** The **Event Stream** layer. It acts as a buffer, storing messages from SNS to ensure that even if the consumer is busy, no data is lost.
4.  **AWS Lambda:** The **Event Consumer**. It is triggered by SQS, processes the incoming data (business logic), and performs the transformation.
5.  **S3 Bucket (Target):** The **Event Sink**. The final processed data is stored here.

---

## 🚀 Key Features

* **Decoupled Services:** Using SQS/SNS ensures that the source and target systems don't depend directly on each other.
* **Scalability:** AWS Lambda scales automatically based on the number of messages in the SQS queue.
* **Reliability:** SQS provides message persistence, ensuring data is not lost during transient failures.
* **Serverless:** No servers to manage; you only pay for what you use.

---

## 📂 Repository Structure

* `lambda_function.py`: The core Python logic for processing the data.
* `s3-to-lambda.py`: Infrastructure or utility script for the pipeline.
* `stock_pricing.csv`: Sample data file for testing.
* `architecture-diagram.png`: Visual representation of the workflow.

---

## 🛠️ How to Setup

1.  **Create S3 Buckets:** Create two buckets (Source and Target).
2.  **Setup SNS Topic:** Create an SNS topic and configure S3 to send "All object create events" to this topic.
3.  **Setup SQS Queue:** Create a Standard SQS queue and subscribe it to the SNS topic.
4.  **Deploy Lambda:** * Upload `lambda_function.py`.
    * Set the SQS queue as the trigger for the Lambda function.
    * Ensure the Lambda IAM Role has permissions to read from SQS and write to the Target S3 bucket.
5.  **Test:** Upload `stock_pricing.csv` to the source bucket and check the target bucket for the output.

---
