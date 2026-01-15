# EC2 Instance Launch with Enforced Tagging Policy

## Project Overview
This project demonstrates how to enforce **mandatory tagging** for Amazon EC2 instances using **AWS IAM policies**. The implementation ensures that EC2 instances cannot be launched unless all required identification tags are provided at launch time, supporting governance, accountability, and cost management best practices.


## Objective
- Enforce mandatory tags during EC2 instance creation  
- Prevent EC2 launches when required tags are missing  
- Validate policy behavior using success and failure scenarios  


## Mandatory Tags
The following tags must be provided while launching an EC2 instance:

| Tag Key | Example Value      |
|---------|--------------------|
| Name    | Onkar              |
| emailID | onkarlad@gmail.com |
| phoneNo | 7XXXXXXXXX         |
| Place   | Pune               |



## Services Used
- Amazon EC2  
- AWS IAM  
- IAM Policies  

---

## How It Works
1. A dedicated IAM user is created for EC2 launches.  
2. A custom IAM policy enforces mandatory tags.  
3. EC2 launch without tags is denied.  
4. EC2 launch with all required tags is allowed.  


## Validation
-  EC2 launch without tags → **Denied**  
-  EC2 launch with mandatory tags → **Successful**  



## Documentation
For detailed implementation steps, screenshots, and explanations, refer to:

📄 **[Report.md](Report.md)**



## Key Learning
- Understanding AWS tagging best practices  
- Implementing governance using IAM explicit deny policies  
- Hands-on experience with real-world AWS controls  

