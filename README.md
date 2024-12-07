# Smart-shop
###Building a Scalable Web Infrastructure for Smart shop

SmartShop is a fictional mid-sized retail company that has recently decided to expand its 
business by launching an online store. As they anticipate an increase in web traffic due to 
marketing campaigns and seasonal sales, SmartShop requires a robust and reliable web 
infrastructure to ensure smooth customer experiences. With minimal in-house IT staff and a limited 
budget, they seek a cost-effective solution that can be manually adjusted for growth as needed, 
providing high availability and essential security while keeping operational complexity low.

Project Goal
The project aims to empower SmartShop to successfully launch and maintain their online store by building a reliable, secure, and scalable AWS-based web infrastructure. This ensures customer satisfaction, data protection, and alignment with the company’s growth goals, all while adhering to budget constraints and operational simplicity.


Technologies Used
•	AWS (Amazon Web Services): The cloud platform for hosting and managing the infrastructure.
•	Amazon EC2: For running virtual machines to host the web application.
•	Amazon VPC (Virtual Private Cloud): To create a secure, isolated network environment for the infrastructure.
•	Apache Web Server: Used for hosting the SmartShop web application.
•	Security Groups: For controlling inbound and outbound traffic to and from the EC2 instances.
•	Network Access Control Lists (NACLs): To provide additional security at the subnet level.
•	IAM (Identity and Access Management): For managing permissions and access control

###Step-by-Step Execution

##CONFIGURING a VIRTUAL PRIVATE CLOUD  (VPC)
Objective:
Set up a secure, isolated, and customizable network environment on AWS to host SmartShop's scalable web infrastructure.

###Steps to Configure the VPC
Create a VPC:
Navigate to the AWS Management Console and select the VPC service.
Click on Your VPCs, and at the top-right corner of the page, select Create VPC.
Choose the VPC and More option.
In the Name tag field, enter a name of your choice (e.g., Lita_project-vpc).
For IPv4 CIDR, ensure the default Manual option is enabled, and enter an IP CIDR block of your choice (e.g., 10.0.0.0/16).
Under Number of Availability Zones, the default is 2. Since this setup involves launching only one instance, reduce it to 1.
For Number of Public and Private Subnets, leave the default value of 1 unchanged.
Set NAT Gateway and VPC Endpoint to None.
Click Create to finalize the configuration.![Lita vpc](https://github.com/user-attachments/assets/594130bd-cf1c-4e59-8ac4-56dd6cd45d5f)


###Alternative Method to Create a VPC
If you prefer not to create everything at once:

Instead of selecting VPC and More, click on VPC Only.
By default, the IPv4 CIDR Manual option is enabled. Enter an IP CIDR block of your choice (e.g., 10.0.0.0/16).
Click Create to finalize the VPC creation.

If you chose the VPC Only option, proceed to set up the Subnet, Internet Gateway, and Route Table manually.
If you opted for the VPC and More option, you can skip these steps and proceed directly to configuring the Security Groups and Network Access List.

##Set Up Subnets
Create a Public Subnet (For Resources Needing Internet Access):
Navigate to Subnets and click Create Subnet at the top-right corner of the page.
In the VPC ID field, select the VPC you just created (e.g., Smart-shop).
Enter a Subnet Name of your choice (e.g., public-subnet).
Choose an Availability Zone (e.g., eu-north-1a).
Specify an IPv4 Subnet CIDR Block (e.g., 10.0.1.0/24).
Click Create Subnet to finalize.
Create a Private Subnet (For Backend Resources):
Repeat the same steps used to create the public subnet.
Use a different Subnet Name (e.g., private-subnet).
Select a different Availability Zone (e.g., eu-north-1b) for redundancy.
Provide a different IPv4 Subnet CIDR Block (e.g., 10.0.2.0/24).
Click Create Subnet to complete the setup


##Configure an Internet Gateway (IGW)
Navigate to Internet Gateways and click Create Internet Gateway at the top-right corner of the page.
Enter a Name Tag (e.g., smart-shopIGW).
Click Create Internet Gateway.
Once the Internet Gateway is created, click Attach to a VPC at the top-right corner of the page.
Select the VPC you want to attach the gateway to.
Click Attach Internet Gateway to complete the process.
This version keeps the instructions clear and well-organized while staying true to your original format. Let me know if you need further refinements!


##Configure a Route Table
Navigate to Route Tables and click Create Route Table at the top-right corner of the page.
Enter a Name for the route table (e.g., smart-shop-route-table).
Select the VPC you want to associate it with.
Click Create Route Table.
Associate the Route Table with the Public Subnet
In the Actions menu at the top-right of the page, select Edit Subnet Association.
Check the box for the Public Subnet and save the changes.
Add a Route to the Route Table
Scroll to the bottom-right of the page and click Edit Routes.
Add a new route:
Destination: 0.0.0.0/0.
Target: Select the Internet Gateway you created earlier.
Save the changes.


##Security Groups and Network Access Control List (NACL) - Subnet Level
Create a Network ACL
Navigate to Network Access Control List (NACL) and click Create Network ACL at the top-right corner of the page.
Enter a Name for the NACL (e.g., smart-shop-NACL).
Select the VPC to associate it with.
Click Create Network ACL.
Create Separate NACLs for Public and Private Subnets
Create two NACLs:
Public NACL: Name it (e.g., lita_project_public_nacl).
Private NACL: Name it (e.g., lita_project_private_nacl).
For each NACL, click Actions and select Edit Subnet Association.
Associate the Public Subnet with the Public NACL.
Associate the Private Subnet with the Private NACL.
Save the changes.
Configure Inbound and Outbound Rules
Scroll to the bottom-right of the page and click Edit Inbound Rules or Edit Outbound Rules for each NACL.
Add rules with the following:
Choose any Rule Number (e.g., 100).
Ensure the rules allow HTTP and SSH traffic.
Save the changes after defining the rules.
![private nacl](https://github.com/user-attachments/assets/8109f4c8-a6d0-4d19-8967-432178e71504)
![public nacl](https://github.com/user-attachments/assets/e8bb28d1-7fe9-4fc8-a24a-823cfa695642)



##Security Group Configuration
In the AWS Management Console, use the Search Bar at the top-left of the page to search for EC2.
Scroll down to the bottom of the EC2 page and click on Security Groups.
At the top-right corner of the page, click on Create Security Group.
Create the Security Group
Enter a Name for the security group (e.g., Oluwatise-Adetola-litaSG).
Add a Description (e.g., Allow SSH and HTTP traffic).
From the VPC drop-down list, select the VPC you created earlier (e.g., Lita-project-vpc).
Configure Rules
Inbound Rules:
Add rules to allow SSH and HTTP traffic from IPv4 Anywhere.
Outbound Rules:
Leave the default setting to allow all traffic.
Click Create Security Group to complete the setup.
![Security group ](https://github.com/user-attachments/assets/95c334b1-3655-4ad4-9a53-a62d8c4a4187)


##Launching an EC2 Instance on AWS: A Step-by-Step Guide
Navigate to Instances in the AWS Management Console.
At the top-right of the page, click on Launch Instance.
Instance Configuration
Enter a Name for the instance (e.g., oluwatiseadetola_lita).
Choose an Amazon Machine Image (AMI). For this guide, select Amazon Linux 2 AMI.
Select your Instance Type. Here, t2.micro is used.
Key Pair Creation
Create a new Key Pair:
Enter a Key Pair Name (e.g., oluwatiseadetola_kp).
Leave the default settings for Key Pair Type and Private Key Format.
Click on Create Key Pair and ensure the key file downloads to your local system.
Network Settings
Edit the Network Settings:
Use the VPC you created earlier.
Select the Private Subnet to ensure your EC2 instance has internet access.
Enable Auto-assign Public IP.
Under Security Group, select the security group created previously.
Storage Configuration
Leave the Configure Storage settings as they are.
Launch the Instance
Click on Launch Instance.
Wait for the instance to initialize and verify that it passes the 2/2 Status Checks.
![Instance running ](https://github.com/user-attachments/assets/0f3e1043-4880-4111-8640-2fb8cd2fb058)
![instance ](https://github.com/user-attachments/assets/df19f3ff-38b5-4360-9a5d-1ffb2ea4efb1)



##Configuring Apache Web Server on EC2
Connect to the Instance:

Select the instance you created in the previous step and click on Connect at the top-right corner of the page.
Choose the SSH Client tab.
Locate Your Key Pair:

On your local device, find the folder where your key pair file was downloaded.
Right-click on an empty space within that folder and open Git Bash.
Establish an SSH Connection:

Copy the line of code provided under Examples in the SSH Client tab.
Paste the copied code into your Bash terminal and press Enter.
When prompted with the question:
"Are you sure you want to continue connecting (yes/no/[fingerprint])?"
Type yes and press Enter.
A successful connection is indicated when the command prompt changes to display the IP address of your instance.
![ssh](https://github.com/user-attachments/assets/0364d736-bd46-4752-9edf-6ab33eb4caa0)


##Install and Configure Apache:

Run the following commands one by one in the terminal:
bash
Copy code
sudo yum update -y  
sudo yum install httpd -y  
sudo systemctl start httpd  
sudo systemctl enable httpd  
Test the Web Server:

Once Apache is successfully installed, go back to your EC2 instance details.
Copy the Public IPv4 Address of the instance.
Paste the IP address into your web browser to verify if the Apache server is running.
![suceessfully connected](https://github.com/user-attachments/assets/423bc059-387c-441f-96a5-c7ebcc65f6ab)


