# Develeap Bucket Optimizer
## Intro
We wanted to use AWS S3 Intelligent-Tiering feature but there is a requirement for bucket objects to be at least 128KB size. We need bucket objects optimization (for costs needs) also for smaller size and therefore we decided to create this project.

The architecture of this project follows the AWS article below, with the following architecture design: 

![alt text](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/08/02/Fig1-S3-Object.png)

https://aws.amazon.com/blogs/architecture/expiring-amazon-s3-objects-based-on-last-accessed-date-to-decrease-costs/

## Properties
Our main goal is to move objects to cheaper storage classes (for the beginning Standard-IA) if they are:
* Were not accessed in the last 30 days (as a variable, might be changed)
* Were not changed in the last 60 days (as a variable, might be changed)

## Prerequisites before running:
 1. S3 terraform statefile backend - we are storing the project's terraform state in an s3 bucket, as a best practice. We assume that the bucket name will be 'bucket-optimizer-tf-backend'. But you can modify it manually.
 2. Source bucket - the bucket we want to optimize. Without the source bucket this project isn't relevant.
 3. Configure variables required variables
 4. Allow S3 log delivery group on the source ACL - currently it's impossible doing so using terraform as only the bucket owner has the permission to enable s3 server access logs. Therefore, we need to configure it manually BEFORE running this terraform project. Please follow AWS documentation: https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-server-access-logging.html
