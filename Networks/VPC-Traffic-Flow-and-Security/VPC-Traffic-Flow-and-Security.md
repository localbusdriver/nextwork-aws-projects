# VPC Traffic Flow and Security  

## Setup VPC Basics
Here I created a new VPC with the following parameters:

* **Name tag**: NextWork VPC
* **IPv4 CIDR**: 10.0.0.0/16 (default)

> **Revision**: *What is an IP address and why do I need to configure this?*
>> An IP address is like a street address or coordinates for the resource within the network.  

> **Revision**: *What is a CIDR Block?*  
>> CIDR is a way to assign a whole block of IP addresses to a subnetwork.  

### Creating Subnets
Next, I created a subnet.  
* A subnet is a division within a VPC where you can launch AWS resources.

I used the following parameters to create my subnet:
* **VPC ID**: NextWork VPC
* **Subnet Name**: Public 1
* **Availability Zone**: Asia Pacific (Sydney) / ap-southeast-2a (first on the dropdown)
* **IPv4 VPC CIDR block**: 10.0.0.0/16
* **IPv4 subnet CIDR block**: 10.0.0.0/24
* **Auto-assign IPv4 address**: enabled

> Revision: *What does it mean to enable auto-assign IPv4 address?*
>> When you enable auto-assign public IPv4 address for a subnet, any EC2 instance launced in that subnet will automatically receive a public IP address. This makes the instance accessible from the internet without needing to manually assign a public IP.

### Create an internet gateway:
As done in the first part of this project, I created an Internet Gateway and only set one parameter; **Name tag**: NextWork IG.  
I then added attached this internet gateway to the newly created VPC.

> **Revision**: *What does attaching an internet gateway to a VPC mean?*
>> By attaching an internet gateway to a VPC, now all resources in the VPC are able to access the internet. 

# Create a route table
Although there is an internet gateway attached to the VPC, I now have to direct the resources to the internet. For this, I must use a route table.  

Every subnet is a VPC need to be linked to a route table because the route table tells the data where to go. A subnet can only use one route table at a time, but multiple subnets can refer to the same route table.  

Example: If we have a web server (i.e. an EC2 instance) hosting a website in a public subnet and you didn't have a route table, users cannot access your website because your subnet wouldn't know where to take incoming traffic.  


When a subnet's route table has route to an internet gateway, the subnet becomes a **public** subnet, which means it can communicate with the internet.  

When opening a default route table, there were two routes inside the table:
* **Route 0.0.0.0/0 | igw-**: directs traffic to the default internet gateway.
* **Route 172.31.0.0/16 | local**: this helps manage internal traffic within the VPC  

Currently, in the default route table for the NextWork VPC, there is only one route for **10.0.0.0/16**. This route allows traffic within the **10.0.0.0/16** CIDR block to flow within the network.  

Next, I added a new route to the NextWork route table.  
The parameters are as follows:
* **destination**: 0.0.0.0/0
* **target**: igw - NextWork IG

> Why is the *destination* 0.0.0.0/0?
>> 0.0.0.0/0 means all IPv4 addresses! This is creating a default route that sends any traffic that doesn't match more specific routes on your route table.
>>Since the the only other route has a destination of 10.0.0.0/16, this means all traffic that is not bound for another resource within the VPC is bound for the internet gateway!

Then, by saving the previously created subnet (Public 1) to the "Explicit subnet associations" tab, I was able to make the subnet public (connecting it to the internet gateway).

