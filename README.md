---

# **🚀Ansible AWS Project: Automating EC2 Instances**  
This project uses **Ansible** to create **AWS EC2 instances** automatically. It also sets up **passwordless authentication** and includes a script to **shut down only Ubuntu servers**.

---

## **📌 Features**  
✔️ **Create AWS EC2 instances** using Ansible  
✔️ **Set up passwordless SSH authentication**  
✔️ **Store EC2 instance details in an inventory file**  
✔️ **Shut down only Ubuntu instances** with Ansible  

---

## **🔧 Prerequisites**  
Before starting, make sure you have:  
- **AWS Account** (For creating EC2 instances)  
- **AWS CLI** (Configured with your credentials)  
- **Python 3**  
- **Ansible** (`pip install ansible`)  
- **Boto3 & botocore** (`pip install boto3`)  
- **Amazon AWS Collection for Ansible** (`ansible-galaxy collection install amazon.aws`)  

---

## **🚀 Setup & Usage**  

### **1️⃣ Create an IAM User & Access Key**  
1. **Log in to AWS Console** and search for **IAM**  
![Screenshot_1](https://github.com/user-attachments/assets/098bdb6e-5443-44e6-8616-2534c74c2b91)
2. Click on **Users** → **Create User**  
![Screenshot_2](https://github.com/user-attachments/assets/b2f552dd-2e71-431d-9f65-ce10ee887ec6)
3. Enter a **user name**, then click **Next**  
![Screenshot_4](https://github.com/user-attachments/assets/6ff75caf-9661-40fa-8a4c-90cecdb56de3)
4. Select **Attach policies directly**, search for **AmazonEC2FullAccess**, and select it  
![Screenshot_5](https://github.com/user-attachments/assets/4754b3c7-729d-4359-a3a1-6f21dbc7dab5)
5. Click **Next** → **Create User**  
![Screenshot_6](https://github.com/user-attachments/assets/73140176-1638-467c-9003-f7693b88abf9)
![Screenshot_7](https://github.com/user-attachments/assets/5a5f6730-a03b-48cb-8141-e35949faefc1)
6. Click on the newly created user, go to **Security Credentials**, and create an **Access Key**  
![Screenshot_8](https://github.com/user-attachments/assets/b9354c01-ef72-4df4-855e-3337d01d274e)
![Screenshot_9](https://github.com/user-attachments/assets/7e791180-4b8a-45fc-a80c-0a22873bc6ef)
7. Select **Application running outside AWS**, then click **Create Access Key**  
![Screenshot_10](https://github.com/user-attachments/assets/021af479-fa78-4b6f-918a-d8722414cfc0)
![Screenshot_11](https://github.com/user-attachments/assets/978c01b8-6fe1-4b1b-adfb-13030125a2dd)
8. Download the **CSV file** containing the Access Key and Secret Key  

---

### **2️⃣ Store AWS Credentials Securely**  
Since credentials should **never** be hardcoded, store them using **Ansible Vault**:  
Create a password for vault
```bash
openssl rand -base64 2048 > ansiblevault.pass
```
Then add your AWS credentials using the below vault command:
```bash
ansible-vault create group_vars/all/pass.yml --vault-password-file ansiblevault.pass
```
and Add
```yaml
ec2_access_key: YOUR_ACCESS_KEY
ec2_secret_key: YOUR_SECRET_KEY
```
---

### **3️⃣ Run the EC2 Playbook**  
Run the playbook to create EC2 instances:  
```bash
ansible-playbook ec2_playbook.yml --vault-password-file ansiblevault.pass
```
This will:  
- Create **2 Ubuntu instances**  
- Create **1 AWS Linux instance**  

---

### **4️⃣ Setup Passwordless SSH**  
For each EC2 instance, run:  
```bash
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
```

---

### **5️⃣ Create an Inventory File**  
After setting up passwordless SSH, create a **`inventory.ini`** file and add your instance IPs:  
```
ec2-user@PublicIP
ubuntu@PublicIP
ubuntu@PublicIP

```
---

### **6️⃣ Shutdown Ubuntu Instances**  
Run the playbook to **shut down only Ubuntu instances**:  
```bash
ansible-playbook -i inventory.ini shutdown.yml --vault-password-file ansiblevault.pass
```

---
