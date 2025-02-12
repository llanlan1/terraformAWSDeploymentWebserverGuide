# Terraform AWS Web Server

This repository contains a script and a simple, step-by-step guide I've made, using Terraform to provision an EC2 instance on AWS and deploy an NGINX Web Server.



# Prerequisites

Please do the following before proceeding:

-Install AWS CLI on your computer/localhost
-Setup an AWS account
-Install & Setup Ubuntu/Linux (Tested on Ubuntu 18.04.6 LTS)
-Install & Configure Git



# Steps


1. Do this if you have yet to setup a user in AWS IAM:

In your AWS account, search and navigate to IAM.
On the left sidebar, under Access Management, click/tap on Users
Create User, add new username, click next, In Permissions Options select Attach policies directly.
Under Permissions policies, search for AmazonEC2FullAccess and check it, then click next, and create user.
It should provide you with both the access key and the secret key.
IMPORTANT: Do not navigate away from the page until you note them down somewhere private. You will need them for later.


2. Run Ubuntu (This will open up the CLI)


3. Install Terraform by running these commands:

sudo apt update && sudo apt install -y software-properties-common

#Add the HashiCorp GPG key:

wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg

#Add the HashiCorp repository:

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list

#Update & install Terraform:

sudo apt update && sudo apt install -y terraform

#Verify installation:

terraform -version  # Output should be something like: Terraform v1.x.x


4. Create a project directory

E.g.:
mkdir terraform-aws-webserver

then
cd terraform-aws-webserver


5. Initialise a Git Repository

git init
echo ".terraform/" > .gitignore


6. Copy the script in the main.tf file in this github repo


7. Create a file (you can call it main.tf)

E.g.
touch main.tf
vim main.tf
type i
paste the script
hit esc key and type:wq 


8. Run this: aws configure

It will prompt you to enter your access key, then your secret access key.
Next, enter your region (normally the default region your account is in, e.g. us-east-1)
Default output format: Leave it blank for default and hit enter.


9. Run these Terraform commands:

terraform init
terraform plan
terraform apply -auto-approve
**It should output a public IP

Your EC2 Instance has been created.


10. Go to your EC2 page in AWS,

On the left sidebar, scroll down and click on Security Groups under Network & Security
Click on the security group that has been created for you just
Click on Edit inbound rules
Click Add rule, type - select HTTP, type - select TCP, port range - 80, source - leave it as custom/default setting, beside it select 0.0.0.0/0 from the drop down
Click Save rules. 
Go back to the Security Groups page, click on Outbound rules > Edit outbound rules
Click Add rule, type - select SSH, type - select TCP, port range - 22, source - leave it as custom/default setting, beside it select 0.0.0.0/0 from the drop down
Click Save rules. 

** Selecting 0.0.0.0/0 is not very secure; once you've tested for successful deployment you can change and limit this to the user's IP 


11. Open your Internet browser, type http://your-public-ip-from-step-9 and hit enter

You should see 'Welcome to nginx!' page
Your webserver has been successfully deployed.



# Clean-up

Run this command:

terraform destroy -auto-approve



