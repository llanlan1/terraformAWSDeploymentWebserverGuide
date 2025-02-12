# Terraform AWS Web Server

This repository contains a script and a simple, step-by-step guide I've made, using Terraform to provision an EC2 instance on AWS and deploy an NGINX Web Server.

---

## Prerequisites

Please do the following before proceeding:

- Install AWS CLI on your computer/localhost
- Setup an AWS account
- Install & Setup Ubuntu/Linux (Tested on Ubuntu 18.04.6 LTS)
- Install & Configure Git

---

## Steps

### **1. Create an IAM User in AWS (If Not Already Done)**

1. In your AWS account, search and navigate to **IAM**.
2. On the left sidebar, under **Access Management**, click **Users**.
3. Click **Create User**, add a new username, then click **Next**.
4. In **Permissions Options**, select **Attach policies directly**.
5. Under **Permissions policies**, search for `AmazonEC2FullAccess` and check it.
6. Click **Next**, then create the user.
7. Note down the **Access Key** and **Secret Key** securely before navigating away from the page.

---

### **2. Run Ubuntu (CLI will open)**

---

### **3. Install Terraform**

Run the following commands:

```sh
sudo apt update && sudo apt install -y software-properties-common
```

**Add the HashiCorp GPG key:**
```sh
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

**Add the HashiCorp repository:**
```sh
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

**Update & Install Terraform:**
```sh
sudo apt update && sudo apt install -y terraform
```

**Verify installation:**
```sh
terraform -version  # Output should be something like: Terraform v1.x.x
```

---

### **4. Create a Project Directory**

```sh
mkdir terraform-aws-webserver
cd terraform-aws-webserver
```

---

### **5. Initialise a Git Repository**

```sh
git init
echo ".terraform/" > .gitignore
```

---

### **6. Copy the Script into `main.tf`**

---

### **7. Create a File (`main.tf`)**

```sh
touch main.tf
vim main.tf
```

1. Press `i` to enter insert mode.
2. Paste the script.
3. Press `ESC`, then type `:wq` to save and exit.

---

### **8. Configure AWS CLI**

Run:
```sh
aws configure
```

- Enter **Access Key** and **Secret Key**.
- Enter **Region** (e.g., `us-east-1`).
- Leave **Default Output Format** blank and hit Enter.

---

### **9. Run Terraform Commands**

```sh
terraform init
terraform plan
terraform apply -auto-approve
```

- This will output a **Public IP** for your EC2 instance.

---

### **10. Configure Security Groups in AWS**

#### **Inbound Rules**
1. Go to **EC2 > Security Groups**.
2. Click on the **Security Group** associated with your instance.
3. Click **Edit Inbound Rules** ? **Add Rule**:
   - **Type**: `HTTP`
   - **Protocol**: `TCP`
   - **Port Range**: `80`
   - **Source**: `0.0.0.0/0`
4. Click **Save Rules**.

#### **Outbound Rules**
1. Click **Edit Outbound Rules** ? **Add Rule**:
   - **Type**: `SSH`
   - **Protocol**: `TCP`
   - **Port Range**: `22`
   - **Source**: `0.0.0.0/0`
2. Click **Save Rules**.

**Security Notice**: Using `0.0.0.0/0` is insecure. Restrict it to your specific IP once tested.

---

### **11. Verify Deployment**

Open a browser and go to:
```
http://your-public-ip-from-step-9
```
You should see **'Welcome to nginx!'**

Your web server has been successfully deployed.

---

## **Clean-Up**

To remove all created resources (so as to not incure running costs):

```sh
terraform destroy -auto-approve
