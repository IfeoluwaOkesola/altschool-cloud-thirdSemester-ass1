# AltSchool Cloud Engineering – Third Semester Assignment 1 (Cloud)

## Task 1: S3 + IAM Setup

### Objective  
Host a public website and manage private file storage using Amazon S3, and control access via IAM. The website should be a basic webpage for the CloudLauch project.  

### Steps Completed  
1. Created the following S3 buckets:  
   - `cloudlaunch-private-bucket-ifeoluwa`  
   - `cloudlaunch-site-bucket-ifeoluwa`  
   - `cloudlaunch-visible-only-bucket-ifeoluwa`  

2. Configured **IAM user**: `cloudlaunch-user` with limited access:  
   - ✅ `s3:ListBucket` on all three buckets.  
   - ✅ `s3:GetObject` and `s3:PutObject` on `cloudlaunch-private-bucket-ifeoluwa` only.  
   - ✅ `s3:GetObject` on `cloudlaunch-site-bucket-ifeoluwa` only.  
   - ❌ No `s3:DeleteObject` permissions anywhere.  
   - ❌ No access to `cloudlaunch-visible-only-bucket-ifeoluwa` objects.  

3. Enabled **"User must create a new password at next sign-in"** for the IAM user.  

### Outputs  
- **S3 Static Website URL:** [http://cloudlaunch-site-bucket-ifeoluwa.s3-website-eu-west-1.amazonaws.com/]  


### IAM Policy JSON  
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListAllBuckets",
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Sid": "SiteBucketRead",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-site-bucket-ifeoluwa/*"
        },
        {
            "Sid": "PrivateBucketReadWrite",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-private-bucket-ifeoluwa/*"
        },
        {
            "Sid": "PrivateBucketList",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-private-bucket-ifeoluwa"
        },
        {
            "Sid": "VisibleBucketListOnly",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-visible-only-bucket-ifeoluwa"
        },
        {
            "Sid": "SiteBucketList",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-site-bucket-ifeoluwa"
        }
    ]
}


# Task 2: VPC Design for CloudLaunch Environment

## Objective
Create a secure and logically separated network environment for future expansion 

---

## VPC Configuration
- **VPC Name:** `cloudlaunch-vpc`  
- **CIDR Block:** `10.0.0.0/16`

---

## Subnets
- **Public Subnet** → `10.0.1.0/24` → Future load balancers / public services.  
- **Application Subnet** → `10.0.2.0/24` → Private app servers.  
- **Database Subnet** → `10.0.3.0/28` → Private RDS-like DB.  

---

## Internet Gateway
- Created **`cloudlaunch-igw`** and attached to VPC.  

---

## Route Tables
- **`cloudlaunch-public-rt`** → Associated with **Public Subnet**.  
  - Default route `0.0.0.0/0` → Internet Gateway.  

- **`cloudlaunch-app-rt`** → Associated with **App Subnet** (no internet access).  

- **`cloudlaunch-db-rt`** → Associated with **DB Subnet** (no internet access).  

---

## Security Groups
- **`cloudlaunch-app-sg`** → Allows inbound HTTP (**port 80**) **within the VPC only**.  
- **`cloudlaunch-db-sg`** → Allows MySQL (**port 3306**) **from App Subnet only**.  

---

## IAM Permissions (for `cloudlaunch-user`, VPC)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeVpcAttribute",
        "ec2:DescribeSubnets",
        "ec2:DescribeRouteTables",
        "ec2:DescribeInternetGateways",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}


## 🌐 AWS Account Access

- **Console login URL:** [https://916863632865.signin.aws.amazon.com/console]  
- **IAM Username:** `cloudlaunch-user`  
- **Password:** `<AltschoolAss1>` *(Password reset enforced at first login ✅)*

