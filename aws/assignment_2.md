# Assignment 2 – Application Load Balancer

## Overview

For this assignment, I deployed two EC2 instances behind an **AWS Application Load Balancer (ALB)**.

The objective was to understand how an ALB distributes HTTP traffic between EC2 instances, performs health checks, and uses Security Groups to control access between the load balancer and backend servers.

---

## Architecture

```text
                              Internet
                                  │
                                  │ HTTP :80
                                  ▼
                     ┌────────────────────────┐
                     │   Application Load     │
                     │       Balancer         │
                     │      test-ALB           │
                     │        ALB-SG           │
                     └───────────┬────────────┘
                                 │
                                 │ HTTP :80
                                 ▼
                         ┌────────────────┐
                         │  Target Group  │
                         │   Health: /    │
                         └───────┬────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
             ┌─────────────┐           ┌─────────────┐
             │   EC2 #1    │           │   EC2 #2    │
             │   Apache    │           │   Apache    │
             │   EC2-SG    │           │   EC2-SG    │
             └─────────────┘           └─────────────┘
```

### Design

The ALB is the **public entry point** to the application.

Traffic follows this path:

```text
Internet
   ↓
ALB :80
   ↓
Target Group
   ↓
Healthy EC2 instance :80
```

The EC2 instances are not configured to accept HTTP traffic directly from the internet.

---

## What I Built

* Two EC2 instances running Apache HTTP Server
* Internet-facing Application Load Balancer
* ALB deployed across two public subnets
* Target Group with both EC2 instances registered
* HTTP listener on port `80`
* Health check configured on `/`
* Separate Security Groups for the ALB and EC2 instances
* Different web-server content to identify backend responses

---

## Web Server Setup

Apache was installed using EC2 user data:

```bash
#!/bin/bash

yum update -y
yum install httpd -y

systemctl enable httpd
systemctl start httpd

echo "<h2>Server running on $(hostname -f)</h2>" > /var/www/html/index.html
```

Using `$(hostname -f)` allows the hostname of the EC2 instance serving the request to be displayed.

---

## Security

Two Security Groups were used.

### ALB Security Group

```text
HTTP :80
Source: 0.0.0.0/0
```

This allows users on the internet to access the ALB.

### EC2 Security Group

```text
HTTP :80
Source: ALB-SG
```

The EC2 instances only accept HTTP traffic originating from the ALB Security Group.

```text
Internet
    │
    ▼
 ALB-SG
    │
    │ HTTP :80
    ▼
 EC2-SG
    │
    ├── EC2 #1
    └── EC2 #2
```

This demonstrates **Security Group referencing** and prevents direct HTTP access to the backend instances.

---

## Target Group & Health Checks

The Target Group uses:

```text
Target type: Instance
Protocol: HTTP
Port: 80
Health check path: /
```

Both registered EC2 instances are currently showing:

```text
2 Healthy
0 Unhealthy
```

This confirms that the ALB can successfully reach the Apache web servers.

### Target Group

![Target Group](screenshots/target-group.png)

---

## Application Load Balancer

The ALB is configured as:

```text
Type: Application
Scheme: Internet-facing
IP address type: IPv4
Listeners: HTTP :80
Availability Zones:
  - eu-west-2a
  - eu-west-2b
```

![Application Load Balancer](screenshots/alb-details.png)

---

## Security Groups

The project uses two dedicated Security Groups:

```text
ALB SG
└── Allow HTTP from anywhere

EC2 SG
└── Allow HTTP only from ALB SG
```

![Security Groups](screenshots/security-groups.png)

---

## Testing

I tested the application using the DNS name provided by the ALB:

```text
http://test-alb-24028399.eu-west-2.elb.amazonaws.com
```

The ALB successfully returned the web application hosted on the EC2 instances.

Example response:

```text
Server running on ip-172-31-1-21.eu-west-2.compute.internal
```

![ALB Application Test](screenshots/alb-test.png)

---

## Key Learnings

This assignment helped me understand:

* How an Application Load Balancer works
* How Target Groups connect an ALB to EC2 instances
* How ALB health checks work
* How Security Group referencing can isolate backend servers
* How an ALB provides a single public entry point
* How multiple EC2 instances can sit behind a load balancer

The main architecture principle I learned is:

> **Expose the load balancer to the internet, not the backend application servers.**

---

## Bonus Tasks

The main assignment is completed. The optional bonus tasks are still pending.

* [ ] Route 53 Alias record pointing to the ALB
* [ ] HTTPS listener using AWS Certificate Manager (ACM)
* [ ] Auto Scaling Group behind the ALB

---

## Project Status

**Completed — Bonus tasks pending**

This assignment was a practical step towards understanding **load balancing, health checks, security-group isolation and highly available AWS application architecture**.
