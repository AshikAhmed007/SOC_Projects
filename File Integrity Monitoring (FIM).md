File Integrity Monitoring (FIM)
Objective

The purpose of this lab is to implement and analyze File Integrity Monitoring (FIM) using Wazuh. FIM helps detect unauthorized file changes, ensures system security, and enables proactive monitoring through continuous verification of critical files and directories.

Environment Setup
Wazuh Manager: Installed on server
Wazuh Agent: Installed on endpoint
Operating System: Ubuntu Linux
Configuration Access: /var/ossec/etc/ossec.conf (editable via sudo nano)
Configuration Steps
1. Accessing Wazuh Server
ssh wazuh-user@<server-ip-address>
2. Editing FIM Configuration on Wazuh Manager

Open the Wazuh configuration file:

sudo nano /var/ossec/etc/ossec.conf

Enable global logging:

<global>
    <logall>yes</logall>
    <logall_json>yes</logall_json>
</global>
3. Configuring FIM on Wazuh Agent

On the agent machine, navigate to the configuration file:

sudo nano /var/ossec/etc/ossec.conf

Enable syscheck (FIM) and define directories for monitoring:

<syscheck>
    <disabled>no</disabled>
    <frequency>43200</frequency> <!-- Executes every 12 hours -->
    <scan_on_start>yes</scan_on_start>

    <!-- Directories to monitor -->
    <directories>/etc,/usr/bin,/usr/sbin,/bin,/sbin,/boot</directories>
    <directories check_all="yes" report_changes="yes" realtime="yes">/home/username/Desktop</directories>
</syscheck>
4. Restarting Wazuh Agent

Apply changes by restarting the agent:

sudo systemctl restart wazuh-agent
Testing the Configuration
1. File Creation
touch /etc/testfile.txt
2. File Modification
echo "test" >> /etc/testfile.txt
3. File Deletion
rm /etc/testfile.txt
Alert Detection
Access the Wazuh Dashboard
Navigate to Security Events → FIM
Verify alerts for:
File added
File modified
File deleted
