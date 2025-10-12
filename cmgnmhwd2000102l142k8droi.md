---
title: "Setting Up Password-less SSH Authentication (Day X of My DevOps Journey"
datePublished: Sun Oct 12 2025 11:30:36 GMT+0000 (Coordinated Universal Time)
cuid: cmgnmhwd2000102l142k8droi
slug: setting-up-password-less-ssh-authentication-day-x-of-my-devops-journey

---

🔐 Enabling Password-less SSH Access Between Jump Host and App Servers

**What I Did:**  
Today I learned how to configure password-less SSH access from the jump host to all app servers. The goal was to let the `thor` user connect to remote servers (stapp01, stapp02, stapp03) without entering passwords, so automated scripts can run smoothly.

**Steps I Followed:**

1. Logged in as `thor` using `sudo su - thor`.
    
2. Generated a new SSH key with `ssh-keygen -t rsa`.
    
3. Copied the public key to each app server using `ssh-copy-id tony@stapp01`, `ssh-copy-id steve@stapp02`, and `ssh-copy-id banner@stapp03`.
    
4. Verified by SSH-ing into each server without a password prompt.
    
5. Ensured `.ssh` permissions were correctly set.
    

**What I Learned:**

* Password-less SSH uses **public/private key pairs** for secure access.
    
* This method is essential for **automation tools** like Ansible and Jenkins.
    
* It eliminates manual password entry and increases efficiency while maintaining security.
    

**Commands Used:**

```plaintext
sudo su - thor
ssh-keygen -t rsa
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
ssh tony@stapp01
```

**Result:**  
Now `thor` can SSH into all app servers password-free — a key step toward full automation in DevOps workflows. 🚀