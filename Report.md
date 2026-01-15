# EC2 Instance Launch with Enforced Tagging Policy Using AWS Tag Policies

## Introduction
In cloud environments, proper resource management and cost control are very important. Organizations use **resource tagging** to identify ownership, track costs, and maintain governance. This project demonstrates how to enforce **mandatory tagging** while launching Amazon EC2 instances so that no instance can be created without required identification tags.

## Project Objective 
The objective of this project is to:
- Understand and apply AWS tagging best practices
- Enforce mandatory tags during EC2 instance launch
- Prevent EC2 instance creation when required tags are missing
- Validate policy behavior using success and failure scenarios

## Required Tags Definition

Every EC2 instance must have the following tags at launch time:

| Tag Key  | Example Value      |
|----------|--------------------|
| Name     | Onkar              |
| emailID  | onkarlad@gmail.com |
| phoneNo  | 7XXXXXXXX          |
| Place    | Pune               |

These tags help in identification, accountability, and resource grouping.

## Services Used

- Amazon EC2
- AWS IAM
- IAM Policies
- AWS Management Console


#### - Why Tagging Is Important

- Cost tracking & billing
- Ownership identification
- Resource organization
- Compliance & governance
- Automation & reporting


## Architecture Overview

The architecture consists of:
- A normal IAM user (non-root)
- A custom IAM policy enforcing mandatory tags
- Amazon EC2 service
- Policy validation before EC2 instance creation

If the required tags are missing, the IAM policy explicitly denies the EC2 launch request.
## Architecture:

```bash
            IAM User (ec2-launch-user)
                      |
                      v
            IAM Tag Enforcement Policy
                      |
                      v
               EC2 Launch Request
                  /        \
                 /          \
            No Tags      All Tags
               |             |
             DENY          ALLOW
               |             |
          EC2 Failed   EC2 Running
```

## Implementation Steps
### Step 1. Create Tag Enforcement IAM Policy
A custom IAM policy was created to deny EC2 instance creation if required tags are missing.

- Go to **AWS Console**
- Search for **IAM**
- Click on **Policies**
- Clcik **Create policy**
- Select **JSON**
**Policy JSON:**
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowEC2RunInstances",
      "Effect": "Allow",
      "Action": "ec2:RunInstances",
      "Resource": "*"
    },
    {
      "Sid": "DenyCreateTagsIfMandatoryTagsMissing",
      "Effect": "Deny",
      "Action": "ec2:CreateTags",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "ForAnyValue:StringNotEquals": {
          "aws:TagKeys": [
            "Name",
            "emailID",
            "phoneNo",
            "Place"
          ]
        }
      }
    }
  ]
}

```
- This policy ensures that EC2 instances cannot be launched unless all required tags are provided.
- Click on Next 
- Give name to policy- `EC2-Enforce-Tag-policy`
- Click on create and here is your policy
![alt text](<Img/Screenshot 2026-01-15 165408.png>)

### Step 2. Create a Normal IAM User
A dedicated IAM user named `IAM user` was created with AWS Management Console access.  
This user is used to test tag enforcement and is not given administrator access.
- Go for **IAM** 
- Click **Users** to create user
- Give User name - `IAM user`
- Give AWS Management Console access
- Attach Required Permissions
- Select `Attach policies directly`
- Attach policies:
```bash
- AmazonEC2FullAccess  # to allow ec2 launch
- EC2-Enforce-Tag-policy  # to enforce tags
```
- Click on create user
 ![alt text](<Img/Screenshot 2026-01-15 165747.png>)
 ![alt text](<Img/Screenshot 2026-01-15 165850.png>)

### Step 3. Login Using IAM User
- Logout from root/admin
- Login as ` IAM user `

## Step 4.Testing and Validation

### Test Case 1: Lauch EC2 Instance WITHOUT Tags 
### Action:
- EC2 → Launch instance
- Choose AMI & instance type
- Skip Tags section
- Click Launch
### Result:
- Instance launch failed.
- Error message indicated an explicit deny due to IAM policy.

### Conclusion:
- The policy correctly blocked EC2 creation.

Screenshot:
![alt text](<Img/Screenshot 2026-01-15 170423.png>)

### Test Case 2: Launch EC2 Instance WITH Required Tags
### Action:
- EC2 → Launch instance
- Scroll to Tags
- Click Add tag
- Add ALL required tags (Resource type = Instance):
- Launched an EC2 instance with all required tags:
  - Name
  - emailID
  - phoneNo
  - Place
![alt text](<Img/Screenshot 2026-01-15 185105.png>)

### Result:
- EC2 instance launched successfully.
- Instance entered the running state.

### Conclusion:
- The policy allowed instance creation when all mandatory tags were provided.

Screenshots:
![alt text](<Img/Screenshot 2026-01-15 185602.png>)
![alt text](<Img/Screenshot 2026-01-15 185651.png>)

## Observations

- IAM policies can be used effectively to enforce governance rules.
- Explicit deny policies ensure strict compliance.
- Tag enforcement helps prevent unmanaged and untraceable cloud resources.

## Conclusion

This project successfully demonstrated how to enforce mandatory tagging for Amazon EC2 instances using AWS IAM policies. By testing both successful and failed launch scenarios, the implementation proved that tagging policies are an effective way to ensure accountability, cost management, and standardization in AWS environments.

## Future Enhancements

- Implement tag enforcement using AWS Organizations SCPs
- Enforce tagging through AWS CLI
- Integrate cost allocation reports using tags
- Apply similar enforcement to other AWS resources