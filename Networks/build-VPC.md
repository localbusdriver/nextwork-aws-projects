# Creating a Virtual Private Cloud (VPC) with AWS

## What is a VPC?
If we image a AWS Region as a country, a VPC is like managing your own city within that country. You can design your own neighbourhoods (aka subnets), traffic rules, and security measures to control how the different resources inside your VPC, like EC2 instances and S3 buckets, are connected and work together. Your city is isolated from other cities i.e. other AWS Accounts' VPCs, giving you full privacy and control over your VPC's layout and rules.  

## Why a VPC?
Imagine if every file in Google Drive from any account was put into the same folder with no subfolders. You'd have to find your files in a chaotic sea of everything uploaded by everyone, making it nearly impossible to manage your files! That's what managing your resources would be like without VPCs.  

## The default VPC in my account
Upon opening the VPC console, and selecting the Sydney region, I found there was already a VPC in the console. AWS provides a default VPC in each AWS account to help you quickly get started! This default VPC allows you to launch resources and test AWS services without needing to set up a VPC from scratch. In other words, the VPC this default VPC is the reason I am able to privately deploy resources like EC2 instances and even connect AWS services from the moment I created my AWS account!  
This default VPC is a handy starting point, especially for beginners. Creating new VPCs allow for more customization like stric security measures, etc.  
Without this default VPC, we could use some AWS resources because some resources do not need a private network to operate. For example, you can store files in S3 (Simple Storage Service) or use AWS Lambda without a VPC.  
What differentiates a VPC from the internet is that a VPC is like an area in the cloud that you own which is not directly within the internet. However, you can connect your VPC to the internet through a gate, called an internet gateway.  

## Creating VPC
