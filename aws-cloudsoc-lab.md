# AWS Cloud SOC Lab practice - credentional comprise, detection and response

## Overview
This project is meant to simulate a Cloud SOC investigation by detecting and responding to compromised aws iam credentials. The lab demonstrates the start of suspicias API activity, detection through AWS guard duty, investigation through cloudtrail logs, and then fixing the problem with iam security controls. 

## Components Used

Attacker workstation (Kali Linux / AWS CLI)

Compromised IAM User (soc-victim)

AWS CloudTrail (API logging)

AWS GuardDuty (threat detection)

IAM (access control and containment)

## Attack 
Created a low privellage IAM user accout to simmulate compromised credentials. Configured access keys locally using AWS cli and performed API calls
```
aws configure

aws s3 ls
aws iam list-users
aws ec2 describe-instances
aws s3api list-buckets
for i in {1..15}; do aws sts get-caller-identity; done
```
<img width="181" height="41" alt="Screenshot 2026-01-25 223158" src="https://github.com/user-attachments/assets/097141ca-ae6d-4cc1-8abf-c4663c599b3b" />

<img width="244" height="46" alt="Screenshot 2026-01-25 223355" src="https://github.com/user-attachments/assets/9edc12d8-88ae-4783-a4b0-bf38ae066e21" />

<img width="210" height="44" alt="Screenshot 2026-01-25 223245" src="https://github.com/user-attachments/assets/ff06cbdd-488c-4ecf-8fdc-6f265b8d7723" />

<img width="324" height="49" alt="Screenshot 2026-01-25 223409" src="https://github.com/user-attachments/assets/5aa1763b-0464-48df-8d30-d85f1ce8fc88" />

## Detection
GuardDuty was enabled to analyze the CloudTrail logs. After the simulated attack, GuardDuty generated these findings

<img width="953" height="89" alt="Screenshot 2026-01-25 224946" src="https://github.com/user-attachments/assets/f122510e-e722-4f27-892c-eee79ffb2f34" />

The GuardDuty findings were further investigated by going into CloudTrail event history. Filtered logs by Access Key ID

<img width="1130" height="54" alt="Screenshot 2026-01-26 215409" src="https://github.com/user-attachments/assets/3fd1df18-44bc-49db-b5f9-d038357eb69b" />

This shows a timeline of the activity to show the attacker behaviour

<img width="760" height="433" alt="image" src="https://github.com/user-attachments/assets/152e9e87-ef0a-4888-877a-10d98dd911c1" />

<img width="757" height="496" alt="image" src="https://github.com/user-attachments/assets/b64ba21b-8154-4544-ba57-f2e5bffcb409" />


## Containment
After the confirmation of the suspicious activity, there are several steps we can take to ensure further security. 

-Deactivated the compromised IAM access key

-Reviewed root account usage

-Enforced MFA

-Restricted IAM permissions

<img width="1130" height="54" alt="Screenshot 2026-01-26 215409" src="https://github.com/user-attachments/assets/bb6ab09d-03f6-40a4-bd17-473aaca124fe" />

Future improvements could be exporting the logs to a SIEM tool such as splunk, and automating the remediation portion with lambda

## Lessons Learned

This lab showed how quickly cloud environments can be abused once IAM credentials are exposed, even with limited permissions. Repeated `ListBuckets` calls made it clear how attackers automate API enumeration instead of manually clicking around.
GuardDuty was used to show suspicious behavior like Kali Linux–based API calls and root credential usage, and CloudTrail was used for understanding what actually happened. Building a timeline from logs helped separate normal activity from attacker behavior.
This also reinforced the importance of least privilege and avoiding routine root access. Permission boundaries limited what the compromised user could do, and proper logging made investigation and response much easier.

