# File Integrity Monitoring (FIM)

## Objective
The objective of this lab is to implement and analyze **File Integrity Monitoring (FIM)** using Wazuh. FIM helps detect unauthorized file changes, ensures system security, and enables proactive monitoring through continuous verification of critical files and directories.

---

## Environment Setup
- **Wazuh Manager:** Installed on server (IP: `192.168.0.152`)  
- **Wazuh Agent:** Installed on endpoint (Ubuntu, IP: `192.168.0.181`)  
- **Operating System:** Ubuntu Linux  
- **Configuration Access:** `/var/ossec/etc/ossec.conf` (editable via `sudo nano`)  

---

## Configuration Steps

### 1. Accessing Wazuh Server
```bash
ssh wazuh-user@192.168.0.152
```
### FIM Configuration on Wazuh Manager

Open the Wazuh configuration file:

`  sudo nano /var/ossec/etc/ossec.conf   `

Enable global logging:
`<global> `   
`<logall>yes</logall>`    
`<logall_json>yes</logall_json>`
`</global>`

### 3\. Configuring FIM on Wazuh Agent

On the Ubuntu agent, open the configuration file:

`  ssh ubuntu@192.168.0.181sudo nano /var/ossec/etc/ossec.conf   `

Enable **syscheck** (FIM) and define directories for monitoring:

`<syscheck>`    
`<disabled>no</disabled>    `
`<frequency>43200</frequency> `
`<scan_on_start>yes</scan_on_start>    `
`<directories>/etc,/usr/bin,/usr/sbin,/bin,/sbin,/boot</directories> `   
`<directories check_all="yes" report_changes="yes" realtime="yes">/home/username/Desktop</directories></syscheck>`

### 4\. Restarting Wazuh Agent

`   sudo systemctl restart wazuh-agent   `

Testing the Configuration
-------------------------

### File Creation

`   touch /etc/testfile.txt   `

### File Modification

`   echo "test" >> /etc/testfile.txt   `

### File Deletion

`   rm /etc/testfile.txt   `

Alert Detection
---------------

1.  Access the **Wazuh Dashboard**
    
2.  Navigate to **Security Events → FIM**
    
3.  Verify alerts for:
    
    *   File added
        
    *   File modified
        
    *   File deleted
