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
aws sts get-caller-identity
aws ec2 describe-instances
for i in {1..15}; do aws sts get-caller-identity; done
```
