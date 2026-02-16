# AWS Cloud Honeypot Deployment & Threat Analysis

## Overview
This project involved deploying a cloud based honeypot environment using AWS EC2 and T-Pot. The goal of this was to practice using the honeypot seetup to capture real word attacker traffick and to analyze attack patterns and behaviors. The honeypot was intentionally exposed on open ports to be exposed to public internet to attrack malicious traffic, simulating a vunerable system.

## Components Used
AWS EC2 Instance (Ubuntu based)
Security Groups (custom inbound rules to allow attacks))
Monitoring Stack (Elastic, Logstash, Kibana, built into T-Pot)

## Deployment
Launched an EC2 instance in AWS titled tpot-prac
<img width="1494" height="30" alt="Screenshot 2026-02-15 220143" src="https://github.com/user-attachments/assets/1c04045e-f8ed-4f16-930f-a47cf9277107" />

Then in Linux I connected to the EC2 Ubuntu instance

<img width="229" height="25" alt="Screenshot 2026-02-15 220306" src="https://github.com/user-attachments/assets/fe617f7c-18b9-4e27-86be-2af787aae02a" />
<br>
<img width="376" height="17" alt="Screenshot 2026-02-15 220313" src="https://github.com/user-attachments/assets/e229d0a7-26d6-4f5f-9d53-af024f399fd8" />

After connecting I downloaded the T-Pot installer

<img width="551" height="36" alt="Screenshot 2026-02-15 220348" src="https://github.com/user-attachments/assets/fb7fd280-f58d-40ca-bcff-3acb93691859" />
<br>
<img width="132" height="71" alt="Screenshot 2026-02-15 220402" src="https://github.com/user-attachments/assets/470a0c25-d265-4b1b-b0b1-a6eb40c26295" />
<br>
<img width="105" height="24" alt="Screenshot 2026-02-15 220406" src="https://github.com/user-attachments/assets/eeb09810-8cc6-4fb5-8eaa-70d315ea87aa" />

Used `systemctl status` to check if T-Pot was running 

<img width="191" height="23" alt="Screenshot 2026-02-15 220434" src="https://github.com/user-attachments/assets/d0516031-41dc-4df0-98fc-570692c6a761" />
<br>
<img width="430" height="23" alt="Screenshot 2026-02-15 220458" src="https://github.com/user-attachments/assets/39da12a0-ad2f-4fb3-acc2-83345b263914" />

After installing installing tpot, I went back to AWS EC2 Security Groups to open inbound ports allowing for attacker to enter

<img width="1571" height="315" alt="image" src="https://github.com/user-attachments/assets/8333a8c5-97f5-44de-85ab-d1c895f72cf2" />

I rebooted the instance and connected again using the port for Tpot 64297. This was also added to the Security Group to make sure it is restricted and not publicly available.

Connected to T-Pot via browser using https://54.167.126.68:64297

## Threat Analysis and Findings

The honeypot was exposed to the public internet for a few hours. The system began receiving malicious traffic almost immediately.

<img width="1900" height="415" alt="Screenshot 2026-02-15 232009" src="https://github.com/user-attachments/assets/730d155d-de33-45d3-a036-6ffeab42369b" />

<img width="1906" height="553" alt="Screenshot 2026-02-15 232019" src="https://github.com/user-attachments/assets/57c44b74-9323-4115-afda-10e4062bcc59" />

<img width="1906" height="498" alt="Screenshot 2026-02-15 232030" src="https://github.com/user-attachments/assets/4df2798a-26cc-435f-a197-0e7b0fdf0f73" />


Total Events Observed: 30

Tanner: 21

Cowrie: 8

Honeytrap: 1

Most of the interactions were captured by Tanner, showing active web app scanning behaviour. SSH/Telnet brute force attempts were captured by Cowrie, and there was minimal activity observed on Honeytrap.

Port 80 (HTTP) – Highest activity

Port 23 (Telnet) – Secondary activity

Port 443 (HTTPS) – Lower volume

After a bit of research on each of these ports, the attacker activity lines up. HTTP is the most targeted because exposed and most common port. Every organization runs web services on this so it makes sense it gets targeted the most. Telnet is vulnerable due to lack of encryption, however it is a more legacy infrastructure so it is not as commonplace for modern attacks. HTTPS is widely used but becasue the data in transit is encrypted it seems to have less attack traffic. Attackers can still however send malicious requests, bypass authentication, brute force logins, etc.

## Takeaway

Even just running a small EC2 instance for a few hours exposed to the public internet it was scanned within hours, attracted known malicious IPs, experienced active reconnaissance and received automated attack traffic

This project helped me bridge the gap between cloud deployment, threat detection, and blue team analysis. It showed how quickly an unprotected system can get targeted.





