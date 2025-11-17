# AWS EC2 Automation Script for Launch & Terminate

This repository contains a **Python script** that automates the launch of multiple EC2 instances and the automatic termination of those instances after a set period. The script is designed for IAM users with limited permissions (e.g., no `CreateKeyPair` or `DescribeVpcs` access).

## 🧩 What this script does

- Fetches the latest Amazon Linux 2 AMI ID from the AWS Systems Manager (SSM) Parameter Store.  
- Launches a specified number of EC2 instances (in this case, 5) with standard instance type (`t2.micro`) in a specified region (`ap-south-1`) by default.  
- Waits until the instances are in the “running” state and prints their public IP (if assigned).  
- Waits for a configurable delay (by default 180 seconds = 3 minutes).  
- Terminates the launched EC2 instances and waits until they reach the “terminated” state.  

## 🧰 Usage

1. Clone or download this repository.  
2. Ensure you have AWS CLI credentials configured with sufficient permissions (launch & terminate EC2 instances, read SSM parameter store).  
3. Adjust the configuration constants in the script if needed:
   ```python
   REGION = "ap-south-1"            # change region if needed  
   INSTANCE_TYPE = "t2.micro"        # instance type  
   MIN_COUNT = 5                     # number of instances to launch  
   MAX_COUNT = 5  
   TERMINATE_DELAY_SECONDS = 180     # time in seconds before automatic termination  
````

4. Run the script:

   ```bash
   python3 python_automation
   ```

   (Ensure the file is executable or explicitly run through `python3`.)

## ✅ Pre-requisites

* An AWS account with IAM credentials (access key + secret key) configured locally (via `~/.aws/credentials` or environment variables).
* Permission to: `ssm:GetParameters`, `ec2:RunInstances`, `ec2:DescribeInstances`, `ec2:TerminateInstances`, and `ec2:DescribeInstances`.
* Internet access from the host machine to interface with AWS.
* Python 3 installed (tested with e.g., Python 3.8+).
* Boto3 and botocore libraries (`pip install boto3 botocore`) if not already present.

## 🔧 Post-run Tips

* Verify instance launch and termination events in the AWS Console under EC2.
* You can change the `TERMINATE_DELAY_SECONDS` to a longer value if you want longer runtime.
* Make sure the IAM user does **not** have overly broad permissions (e.g., avoid granting `ec2:*`, `iam:*`) if you intend to limit the blast radius.
* Logging could be improved by adding AWS CloudTrail or CloudWatch logs if you extend the script.
* If you want to use a different AMI, you can modify the `get_latest_amzn2_ami` function or supply a static AMI ID.
---
