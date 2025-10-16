---
title: "How I Opened Port 8082 Using Firewalld on Linux Servers"
datePublished: Thu Oct 16 2025 07:48:25 GMT+0000 (Coordinated Universal Time)
cuid: cmgt4bl61000002l75pp7bouj
slug: how-i-opened-port-8082-using-firewalld-on-linux-servers
tags: linux-devops-firewalld-sysadmin-networking

---

## Introduction

As part of managing servers in a real-world DevOps environment, I recently faced a common networking requirement: allowing a web application to communicate through a specific port while keeping the system secure.

On the Nautilus Backup Server, the team rolled out a web UI application that runs on **port 8082**. With `firewalld` active on the server, my task was to allow incoming connections to this port in the **public zone**.

In this blog, I’ll show you exactly how I accomplished this step by step.

---

## The Problem

By default, Linux servers running `firewalld` block incoming connections on non-standard ports.

* Without proper rules, the application on port 8082 wouldn’t be accessible.
    
* Security must be maintained, meaning only the intended port should be open.
    

So, the challenge was:

1. Open **port 8082/tcp** for incoming connections.
    
2. Ensure the **zone is set to public**.
    
3. Make the rule **permanent** and immediately effective.
    

---

## Step-by-Step Solution

### 1️⃣ Check Firewalld Status

First, I confirmed that `firewalld` was running:

```plaintext
sudo firewall-cmd --state
```

Expected output:

```plaintext
running
```

If it wasn’t running, I could start it with:

```plaintext
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

---

### 2️⃣ Verify the Active Zone

Next, I checked which zone was active:

```plaintext
sudo firewall-cmd --get-active-zones
```

Output example:

```plaintext
public
  interfaces: eth0
```

This confirmed that the **public zone** was being used.

---

### 3️⃣ Allow Port 8082/tcp

To allow incoming traffic on port 8082 permanently, I ran:

```plaintext
sudo firewall-cmd --zone=public --add-port=8082/tcp --permanent
```

Explanation:

* `--zone=public` → specifies the zone
    
* `--add-port=8082/tcp` → opens port 8082 for TCP connections
    
* `--permanent` → ensures the rule persists after server reboot
    

---

### 4️⃣ Reload the Firewall

After making permanent changes, I reloaded the firewall to apply them immediately:

```plaintext
sudo firewall-cmd --reload
```

---

### 5️⃣ Verify the Rule

Finally, I verified that port 8082 was open:

```plaintext
sudo firewall-cmd --zone=public --list-ports
```

Expected output:

```plaintext
8082/tcp
```

---

## 💡 What I Learned

* `firewalld` allows fine-grained control over network access using zones and ports.
    
* Always check the active zone before adding rules.
    
* Permanent rules require a reload to take effect immediately.
    

---

## 🚀 Conclusion

Opening a port on Linux with `firewalld` is simple but must be done carefully to maintain security.  
By following the steps above, I successfully allowed the web UI application on **port 8082** to be accessible without compromising the server’s firewall.

This approach can be applied to any TCP/UDP port, helping DevOps engineers configure servers efficiently and securely.