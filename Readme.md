# **Ansible EC2 Nginx Setup**  
Automated Nginx deployment and management on AWS EC2 instances using Ansible.  

---

## **📂 Project Structure**  
```
ec2_nginx_setup/  
├── ansible_user.pem       # SSH key for EC2 access (keep secure!)  
├── hosts.ini              # Ansible inventory file  
├── nginx_install.yml      # Playbook to install & configure Nginx  
├── nginx_remove.yml       # Playbook to completely remove Nginx  
└── README.md              # This documentation  
```  

---

## **🚀 Prerequisites**  
1. **Ansible** installed on your control machine:  
   ```bash
   sudo apt install ansible  # Ubuntu/Debian
   ```
2. **AWS EC2 instances** running Ubuntu/CentOS.  
3. **SSH access** configured with `ansible_user.pem`.  

---

## **🔧 Setup**  

### **1. Configure Inventory (`hosts.ini`)**  
Edit `hosts.ini` to include your EC2 instances:  
```ini
[webservers]
ec2_instance ansible_host=ec2-XX-XX-XX-XX.compute-1.amazonaws.com

[webservers:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=./ansible_user.pem
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### **2. Set Proper Permissions**  
```bash
chmod 400 ansible_user.pem  # Restrict key file access
```

---

## **🛠️ Usage**  

### **Install Nginx**  
```bash
ansible-playbook -i hosts.ini nginx_install.yml
```  
✅ Installs and starts Nginx.  
✅ Configures automatic restart on failure.  

### **Remove Nginx**  
```bash
ansible-playbook -i hosts.ini nginx_remove.yml
```  
⚠️ **Warning**: This playbook **fully uninstalls** Nginx, including configs and logs.  

---

## **🔍 Verification**  
Check if Nginx is running:  
```bash
ansible webservers -i hosts.ini -m shell -a "systemctl status nginx" --become
```  

Access Nginx in your browser:  
```
http://<EC2_PUBLIC_IP>
```

---

## **🚨 Troubleshooting**  

### **SSH Connection Issues**  
- Ensure `ansible_user.pem` is the correct key pair for your EC2 instances.  
- Verify security groups allow **SSH (port 22)** from your IP.  

### **Package Installation Failures**  
- Run manually to debug:  
  ```bash
  ansible webservers -i hosts.ini -m apt -a "name=nginx state=present" --become -vvv
  ```

### **Permission Denied Errors**  
```bash
chmod 400 ansible_user.pem  # Ensure strict permissions
```

---

## **📜 License**  
MIT  

--- 

## **📌 Notes**  
- **Back up configurations** before running `nginx_remove.yml`.  
- For production, use **Ansible Vault** to encrypt sensitive data.  

--- 

