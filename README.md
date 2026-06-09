# LOG-ANALYSIS-USING-WAZUH-SSH-BRUTE-FORCE-Attack-Detection-

## **Objective**  
The objective of this task is to **detect SSH brute-force attacks** on an Kali machine using **Wazuh’s security monitoring capabilities**.
In this project Simulate an SSH brute-force attack, analyze logs, and detect suspicious authentication attempts using Wazuh alerts.

---

## **Configure Wazuh Agent on Kali Target Machine**  

### **Requirements**  
- **Wazuh Server:** Installed and running 
- **Kali (Target Machine).**  
- **Wazuh agent installed on the Kali machine.**  
- **SSH enabled on the target machine.**  
- **Admin or sudo access on the target machine.**  

### **Step 1: Install Wazuh Agent on Kali**  
1. Download and install the **Wazuh Agent** on the target machine:  
```
curl -sO https://packages.wazuh.com/4.7/wazuh-agent-linux.sh && sudo bash wazuh-agent-linux.sh
```
2. Configure the agent to connect to the Wazuh Server:
```
sudo nano /var/ossec/etc/ossec.conf
```
- Replace the IP with the Wazuh Server IP address (Ubuntu)
```
<address>WAZUH-SERVER-IP</address>
```
3. Restart the Wazuh Agent service:
```
sudo systemctl restart wazuh-agent
```

## Attack Simulation & Detection

### Step 1: Simulate an SSH Brute-Force Attack Using Hydra

1. From the attacker machine, install Hydra:

```
sudo apt update && sudo apt install hydra -y
```

2. Run the following command to simulate an SSH brute-force attack:

```
hydra -l kali -P /usr/share/wordlists/rockyou.txt <target-IP> ssh
```
This command attempts multiple password combinations to gain SSH access

- Replace `<target-IP>` with the Kali target machine’s IP address.
- `-l kali` specifies the username to attack.
- `-P /usr/share/wordlists/rockyou.txt` uses a password list for brute-forcing SSH.
  
3. If a valid password is found, Hydra will display:

```
[22][ssh] host: <target-IP> login: kali password: <password>
```

### Step 2: Detect SSH Brute Force Attempts in Wazuh
1. Open the Wazuh Dashboard `(http://<Wazuh-Server-IP>)`
2. Navigate to Security Events and run below query
```
rule.id:(5551 OR 5712)
```
4.Wazuh successfully detected the brute-force activity and generated alerts such as:

- Description: SSH authentication failed 
- Description: Brute-force attack detected 
- Severity Level: High (Level 10)

Analysis:

The system identified repeated login attempts within a short period, which is a clear indication of a brute-force attack. This demonstrates Wazuh’s capability to detect credential-based attacks

## Look for logs indicating:
- Multiple failed SSH login attempts from the same IP.
- High volume of authentication failures in a short period.
- Successful logins after repeated failed attempts (possible compromise).

## Conclusion
✅ Successfully configured Wazuh Agent on an kali machine for SSH monitoring.   
✅ Simulated SSH brute-force attacks using Hydra.   
✅ Detected brute-force attempts in Wazuh logs and alerts.   
✅ Learned how SOC analysts investigate authentication attacks using SIEM tools.  

## Screenshots

https://github.com/Subeen-bit/LOG-ANALYSIS-USING-WAZUH-SSH-BRUTE-FORCE-Attack-Detection-/blob/main/Screenshot%202026-06-09%20222636.png

https://github.com/Subeen-bit/LOG-ANALYSIS-USING-WAZUH-SSH-BRUTE-FORCE-Attack-Detection-/blob/main/Screenshot%202026-06-09%20222712.png
