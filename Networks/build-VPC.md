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
When creating the VPC, I chose VPC only, set the name tag to NextWork *VPC* and set the IPv4 CIDR to *10.0.0.0/16*.  

### What is an IP address and why do I need to configure this?
An IP address is like a street address or coordinates for the resources in the VPC. Configuring IP addresses in the VPC ensures resources can be uniquely identified and located, allowing resources to find and talk to communicate with each other without conflicts.  

IPv4 stands for Internet Protocol version 4, which is the most widely used way to write an IP address. IPv4 addresses are written as four sets of numbers seperated by dots (e.g. 192.168.0.1).  

The number between the dots fall between 0 and 255. This means that IP addresses go from *0.0.0.0* all the way up to *255.255.255.255*! There are a total of 4,294,967,296 possible IPv4 addresses, and in general, two devices cannot share the same IPv4 address in the same network, or within the same VPC.  

Each number between the dots is represented by 8 bits. In binary, 8 bits can represent any number from 0 (00000000) to 255 (11111111). This is why the numbers range from 0.0.0.0 to 255.255.255.255. An entire IPv4 address contains 4 (numbers) * 8 (bits) = 32 bits.

### What is a CIDR block?
CIDR (Classless Inter-Domain Routing) is a way to assign a whole block of IP addresses, kind of like creating a zone/area in a city.  

For example, **10.0.0.0/16**.
**10.0.0.0/16** means the first 16 bits of the IP address (10.0) are fixed but the remaining 16 bits (the last half of the ip address) can be allocated however you like. This means IP addresses within the CIDR block range from *10.0.0.0* to *10.0.255.255*!  
*There are 2^16 (65,536) possible IP addresses within this subnet.*  

