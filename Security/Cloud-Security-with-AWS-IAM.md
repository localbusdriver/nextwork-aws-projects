# Cloud Security with AWS IAM
In this project, I am exploring how I can use AWS Identity and Access Management (IAM) to control who is authenticated and authorized to use my account's permissions.  

I will create an EC2 instance and use IAM to manage who can access and use my resources.  

## First steps
First I created an EC2 instance, in my region (Sydney), with the following configurations:
- Name: *nextwork-production-james*
- Tags:
    - Key: **Env**
    - Value: **production**
- AMI: *Free tier*
- Instance Type: *Free tier*
- Key pair: *Proceed without a key pair*

### What are tags and how will it be useful? 
Tags are like labels that we can attach to AWS resources for organization.  

In this case, I am using a tag called "Env" with a value of "production" or "development" to label the instances used in production vs development environments.  

Tagging allows us to indentify resources with the same tag at once.  


### What is AMI?
Amazon Machine Image (AMI) is a template or blueprint used to create EC2 instances and contains the operating system along with the applications needed to launch the instance.  

### What is instance type?
If AMIs give you pre-built software and operating systems, instance types cover the 'hardware' components. This will determine how fast and powerful the instance performs.

### What is a key pair?
A key pair is primarily used for accessing an EC2 instance securely without going through the AWS Management console. Instead of using the console, it allows us to use SSH (Secure Shell) with the key pairs.  
This is not recommended as it limits our the managers ability to troubleshoot or manage EC2 instances through a secure way outside of the console.  

I have skipped configuring network and storage settings for simplicity in this project. These setting are crucial for fine-tuning EC2 instances' performance, security, and connectivity, but for this project, I will focus on the basic steps of launching instances with minimal configuration.  


Next I created another EC2 instance with the same flow as the previous instance, except I changed the *Env* tag to *development* and changed the name to *nextwork-development-james*.


## Creating an IAM Policy

### What is IAM?
Identity and Access Management (IAM) is used to manage the access level that other users and services have to our resources.  

An IAM Policy is a rule for who can do what with our AWS resources.  

In the IAM Console, I opened the Policy tab and created a new Policy using *JSON*.  

I changed the default JSON to the following:
'''
{    
  "Version": "2012-10-17",    
  "Statement": [        
    {            
      "Effect": "Allow",            
      "Action": "ec2:*",            
      "Resource": "*",            
      "Condition": {                
        "StringEquals": {                    
          "ec2:ResourceTag/Env": "development"                
        }            
      }        
    },        
    {            
      "Effect": "Allow",            
      "Action": "ec2:Describe*",            
      "Resource": "*"        
    },        
    {            
      "Effect": "Deny",     
      "Action": [
        "ec2:DeleteTags",
        "ec2:CreateTags"
      ],            
      "Resource": "*"        
    }    
  ] 
}
'''

This policy allows some actions for instances tagged with "Env = development" while denying the ability to create or delete tags for all instances.  

### Definitions for keywords:
- *Version* (e.g. "2012-10-17")
    - This is the date of the latest policy version. This tells you whether the policy is up to date with the latest standards and practices.
- *Statement*
    - This is the main part of the policy structure and defines a list of permissions.
- *Effect*
    - This can have two values - either **Allow** or **Deny** - to indicate whether the policy allows or denies a certain action.
    - **Deny** has priority.
- *Action*
    - A list of actions that the policy *Allows* or *Denies*.
    - E.g.
'''
{
  "Effect": "Allow",
  "Action": "ec2:*",
  ...
}
'''

    - This means all actions that you could possibly take in EC2 instances are allowed.
- *Resources*
    -  Which resources does this policy apply to? Specifying '*' means all resources in the defined scope.
- *Condition Block (optional)
    - The circumstances under which the policy is in action.
    - E.g.
'''
"Condition": {
  "StringEquals": {
    "ec2:ResourceTag/Env": "development"
  }
}
'''
In this case, the condition is that the resource is tagged **Env - developmemt**. This means that specifying "Resources": "*" in the line above means all resources with the **Env - development** tag are impacted by your statement.


Next, I filled in the policies details:
- Name: **NextWorkDevEnvironmentPolicy**
- Description: IAM Policy for NextWork's development evironment.


## Creating an AWS Account Alias

### What is an Account Alias?
An Account Alias is a friendly name for your AWS account that you can use instead of your account ID to sign in to the AWS Management Console.  

You would create an alias to make it easier to remember (instead of using your account ID) and share the AWS console's login URL with others.

For this step, I created an alias with the name: *nextwork-alias-jamesla*


## Creating IAM Users and User Groups

## What is an IAM user?
IAM users are users or services that are able to interact with AWS resources

### What is an IAM user group?
An IAM user group is a collection/folder of IAM users. It allows us to manage permissions for all the users in your group at the same time by attaching policies to the group rather than individual users.  

For this step, I created a new User Group and configured it with the following details:
- Name: **nextwork-dev-group**
- Attach permission policies: **NextWorkDevEnvironmentPolicy**

Then, I created a new User:
- User name: **nextwork-dev-james**
- Provide user access to Management Console: True

Providing user access to Management Console gives the user access to AWS services through the Console.  

Next, I added this user to the User group. 


## Testing the User's access
Here, I tested the user's access by opening the Console sign-in URL in an incognito browser and logging in.  

Upon entering the Console, I noticed that many dashboard panels were showing *Access Denied*!  
I tried stopping both the deployed EC2 instances and I received an error for the production instance and received a "successful" banner for the development instance.

### IAM Policy Simulator
Manually testing a policy by shutting down an instance is not an effective way to test our policies. For this reason there is an IAM Policy Simulator. This lets you test and validate policies without affecting out actual AWS resources.  

Once I opened the IAM Policy Simulator, I selected the group I had created(*nextwork-dev-group*), selected the EC2 service, and selected the **DeleteTags** and **StopInstances** actions.  

When running this, both tests resulted in "**denied**".  

**Why does *StopInstances* say "denied"?** - The dev user could stop the dev instance in their EC2 Console, so why does it say denied?  
The action was denied because the simulation resource is "*", which means all resources.  

I tried running the simulation again, but this time, in the Instance field, I added *development* to indicated that I want to run the simulation for the instances with that tag.  

Now the simulator tells me that access is granted!   

