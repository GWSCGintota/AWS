# Amazon EC2 in AWS — Comprehensive Notes

## 1. What is Amazon EC2?

**Amazon EC2** stands for **Amazon Elastic Compute Cloud**.

Amazon EC2 is an AWS service that allows users to create and run **virtual servers** in the cloud. These virtual servers are called **EC2 instances**.

In simple words:

> EC2 is like renting a computer/server from AWS instead of buying and maintaining a physical server.

You can use EC2 to run websites, web applications, backend APIs, databases, development environments, batch jobs, and many other workloads.

---

## 2. What is an EC2 Instance?

An **EC2 instance** is a virtual machine running inside AWS.

An EC2 instance can have:

* Operating system
* CPU
* Memory/RAM
* Storage
* Network connection
* Security rules
* Public or private IP address

Example:

```text
EC2 Instance
├── Operating System: Ubuntu / Amazon Linux / Windows
├── CPU: 2 vCPUs
├── RAM: 4 GB
├── Storage: 30 GB EBS volume
├── Network: VPC and subnet
└── Security: Security group rules
```

---

## 3. Why Do We Use EC2?

EC2 is used because it provides flexible computing power in the cloud.

Main advantages:

* No need to buy physical servers
* Servers can be created quickly
* Capacity can be increased or decreased
* Different instance sizes are available
* Users can pay based on usage
* Suitable for small, medium, and enterprise-level applications

Example:

```text
Without EC2:
Buy physical server → Install OS → Configure network → Maintain hardware

With EC2:
Launch instance → Install app → Run service
```

---

## 4. Common Uses of EC2

EC2 can be used for:

| Use Case            | Example                                |
| ------------------- | -------------------------------------- |
| Web hosting         | Hosting a company website              |
| Application hosting | Running backend APIs                   |
| Development/testing | Creating temporary servers for testing |
| Database hosting    | Running MySQL, PostgreSQL, MongoDB     |
| Machine learning    | Training or testing ML models          |
| Batch processing    | Running scheduled data processing jobs |
| File servers        | Storing and sharing internal files     |
| DevOps tools        | Running Jenkins, GitLab, Docker, etc.  |

---

## 5. Main Components of EC2

When launching an EC2 instance, several important components are used.

```text
EC2 Instance
├── AMI
├── Instance Type
├── Key Pair
├── Security Group
├── Storage Volume
├── VPC
├── Subnet
├── Public/Private IP
└── IAM Role
```

---

## 6. Amazon Machine Image — AMI

An **AMI** is a template used to create an EC2 instance.

It contains the software required to start the instance, such as:

* Operating system
* Application server
* Pre-installed software
* System configuration

Examples of AMIs:

* Amazon Linux
* Ubuntu
* Windows Server
* Red Hat Enterprise Linux
* Debian

Example:

```text
If you want a Linux server:
Choose Ubuntu AMI or Amazon Linux AMI

If you want a Windows server:
Choose Windows Server AMI
```

---

## 7. Instance Type

The **instance type** defines the hardware capacity of an EC2 instance.

It decides:

* Number of CPUs
* Amount of RAM
* Network performance
* Storage performance

Example instance types:

| Instance Type | Description                           |
| ------------- | ------------------------------------- |
| t2.micro      | Small instance, suitable for learning |
| t3.micro      | Small general-purpose instance        |
| t3.medium     | Medium general-purpose instance       |
| m5.large      | General production workload           |
| c5.large      | Compute-heavy workload                |
| r5.large      | Memory-heavy workload                 |

Instance type families:

| Family                | Purpose                        |
| --------------------- | ------------------------------ |
| General purpose       | Web servers, small apps        |
| Compute optimized     | CPU-heavy applications         |
| Memory optimized      | Databases, analytics           |
| Storage optimized     | High disk read/write workloads |
| Accelerated computing | GPU, ML, graphics processing   |

---

## 8. Key Pair

A **key pair** is used to securely connect to an EC2 instance.

It contains:

* Public key
* Private key

For Linux EC2 instances, the private key is used to connect using SSH.

Example SSH command:

```bash
ssh -i my-key.pem ubuntu@ec2-public-ip
```

For Amazon Linux:

```bash
ssh -i my-key.pem ec2-user@ec2-public-ip
```

Important points:

* Keep the private key safe.
* Do not share the `.pem` file.
* If the key is lost, connecting to the instance can become difficult.
* Set correct permissions for the key file before using it.

Example:

```bash
chmod 400 my-key.pem
```

---

## 9. Security Group

A **security group** is a virtual firewall for an EC2 instance.

It controls:

* Inbound traffic
* Outbound traffic

### Inbound Rules

Inbound rules control traffic coming **into** the EC2 instance.

Example:

| Type  | Port | Source    | Purpose                     |
| ----- | ---: | --------- | --------------------------- |
| SSH   |   22 | Your IP   | Connect to Linux server     |
| HTTP  |   80 | 0.0.0.0/0 | Allow website access        |
| HTTPS |  443 | 0.0.0.0/0 | Allow secure website access |
| RDP   | 3389 | Your IP   | Connect to Windows server   |

### Outbound Rules

Outbound rules control traffic going **out from** the EC2 instance.

Usually, the default outbound rule allows all traffic.

Example:

```text
All traffic → 0.0.0.0/0
```

### Important Security Advice

Do not open SSH to everyone unless necessary.

Avoid:

```text
SSH 22 → 0.0.0.0/0
```

Better:

```text
SSH 22 → Your IP address only
```

---

## 10. EC2 and VPC

EC2 instances are launched inside a **VPC**.

A **VPC** is a private network in AWS.

Inside a VPC, EC2 instances are placed inside **subnets**.

Example:

```text
VPC: 10.0.0.0/16

Public Subnet: 10.0.1.0/24
└── EC2 Web Server

Private Subnet: 10.0.2.0/24
└── EC2 Database Server
```

---

## 11. EC2 in a Public Subnet

An EC2 instance in a **public subnet** can be accessed from the internet if it has:

* Public IP address
* Route to an Internet Gateway
* Security group allowing required traffic

Example:

```text
Internet
   ↓
Internet Gateway
   ↓
Public Subnet
   ↓
EC2 Web Server
```

Used for:

* Web servers
* Public APIs
* Bastion hosts
* Load balancers

---

## 12. EC2 in a Private Subnet

An EC2 instance in a **private subnet** is not directly accessible from the internet.

Used for:

* Databases
* Application servers
* Internal systems
* Backend services

Example:

```text
Public Subnet
└── Web Server

Private Subnet
└── Database Server
```

The database should normally be in a private subnet for security.

---

## 13. Public IP, Private IP, and Elastic IP

### Private IP

A private IP is used for communication inside the VPC.

Example:

```text
10.0.1.25
```

### Public IP

A public IP allows the instance to be accessed from the internet.

Example:

```text
13.211.45.20
```

A public IP may change when the instance is stopped and started.

### Elastic IP

An **Elastic IP** is a static public IPv4 address.

It is useful when you need a fixed IP address for your EC2 instance.

Example:

```text
Domain name → Elastic IP → EC2 instance
```

---

## 14. EC2 Storage

EC2 instances can use different types of storage.

### 14.1 EBS Volume

**EBS** stands for **Elastic Block Store**.

It is like a virtual hard disk attached to the EC2 instance.

Used for:

* Operating system
* Application files
* Logs
* Databases
* Persistent data

Example:

```text
EC2 Instance
└── EBS Volume: 30 GB
```

### 14.2 Root Volume

The root volume contains the operating system.

Example:

```text
Ubuntu OS installed on root EBS volume
```

### 14.3 Additional EBS Volume

You can attach extra EBS volumes for additional storage.

Example:

```text
Root volume: 30 GB
Additional volume: 100 GB
```

### 14.4 Instance Store

Instance store is temporary storage physically attached to the host machine.

Important:

* Very fast
* Temporary
* Data can be lost when the instance is stopped or terminated

---

## 15. EC2 Instance Lifecycle

An EC2 instance has different states.

| State         | Meaning                      |
| ------------- | ---------------------------- |
| Pending       | Instance is being created    |
| Running       | Instance is active           |
| Stopping      | Instance is shutting down    |
| Stopped       | Instance is turned off       |
| Shutting-down | Instance is being terminated |
| Terminated    | Instance is deleted          |

### Stop vs Terminate

| Action    | Meaning                                      |
| --------- | -------------------------------------------- |
| Stop      | Turns off the instance, can be started again |
| Start     | Starts a stopped instance                    |
| Reboot    | Restarts the instance                        |
| Terminate | Permanently deletes the instance             |

Be careful with **Terminate**, because it may delete the instance and its storage depending on configuration.

---

## 16. User Data

**User data** is a script that runs when an EC2 instance is launched.

It is commonly used to automatically install software.

Example user data script for installing Apache:

```bash
#!/bin/bash
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
echo "Hello from EC2" | sudo tee /var/www/html/index.html
```

This script:

1. Updates the package list
2. Installs Apache web server
3. Starts Apache
4. Enables Apache on boot
5. Creates a simple webpage

---

## 17. IAM Role for EC2

An **IAM role** gives permissions to an EC2 instance.

Instead of storing AWS access keys inside the server, attach an IAM role.

Example:

```text
EC2 needs to read files from S3
→ Attach IAM role with S3 read permission
```

Benefits:

* More secure than storing access keys
* Easier permission management
* Can be changed without editing application code

---

## 18. Connecting to EC2

### 18.1 Connect to Linux EC2 Using SSH

Example:

```bash
ssh -i my-key.pem ubuntu@public-ip-address
```

For Ubuntu:

```bash
ssh -i my-key.pem ubuntu@13.211.45.20
```

For Amazon Linux:

```bash
ssh -i my-key.pem ec2-user@13.211.45.20
```

### 18.2 Connect to Windows EC2 Using RDP

Windows instances are usually accessed through Remote Desktop Protocol.

Port:

```text
3389
```

---

## 19. Common EC2 Ports

| Port | Protocol         | Purpose              |
| ---: | ---------------- | -------------------- |
|   22 | SSH              | Linux remote login   |
|   80 | HTTP             | Web traffic          |
|  443 | HTTPS            | Secure web traffic   |
| 3389 | RDP              | Windows remote login |
| 3306 | MySQL            | MySQL database       |
| 5432 | PostgreSQL       | PostgreSQL database  |
| 8080 | HTTP alternative | App servers          |

---

## 20. EC2 Pricing Options

EC2 has several pricing models.

### 20.1 On-Demand Instances

You pay for what you use.

Good for:

* Short-term workloads
* Testing
* Applications with unpredictable usage

### 20.2 Reserved Instances

You commit for a longer period, usually 1 or 3 years.

Good for:

* Stable workloads
* Production systems
* Long-term applications

### 20.3 Savings Plans

You commit to a certain amount of usage.

Good for:

* Long-term usage
* Flexible instance usage

### 20.4 Spot Instances

Spot Instances use spare EC2 capacity at lower prices.

Good for:

* Batch jobs
* Data processing
* Non-critical workloads
* Fault-tolerant workloads

Important:

> Spot Instances can be interrupted by AWS when capacity is needed.

---

## 21. EC2 Auto Scaling

**EC2 Auto Scaling** automatically increases or decreases the number of EC2 instances based on demand.

Example:

```text
Low traffic:
2 EC2 instances

High traffic:
5 EC2 instances

Traffic decreases:
Back to 2 EC2 instances
```

Benefits:

* Better availability
* Better performance
* Cost control
* Automatic scaling

---

## 22. Load Balancer with EC2

A load balancer distributes incoming traffic across multiple EC2 instances.

Example:

```text
Users
  ↓
Application Load Balancer
  ↓
EC2 Instance 1
EC2 Instance 2
EC2 Instance 3
```

Benefits:

* Improves availability
* Reduces load on one server
* Sends traffic only to healthy instances
* Helps handle high traffic

---

## 23. EC2 Monitoring

EC2 can be monitored using **Amazon CloudWatch**.

CloudWatch can monitor:

* CPU usage
* Network traffic
* Disk activity
* Status checks
* Logs
* Alarms

Example:

```text
If CPU usage > 80% for 5 minutes,
send an alert.
```

---

## 24. EC2 Backup Methods

Common EC2 backup methods:

* EBS snapshots
* AMI backups
* AWS Backup

### EBS Snapshot

An EBS snapshot is a backup of an EBS volume.

### AMI Backup

An AMI can be created from an existing EC2 instance. Later, a new instance can be launched from that AMI.

Example:

```text
Current EC2 Instance
   ↓
Create AMI
   ↓
Launch new EC2 instance from AMI
```

---

## 25. Simple EC2 Web Server Architecture

```text
Internet
   ↓
Internet Gateway
   ↓
Public Subnet
   ↓
EC2 Web Server
   ↓
Security Group
   ├── Allow HTTP 80
   ├── Allow HTTPS 443
   └── Allow SSH 22 from admin IP
```

---

## 26. Example: Hosting a Website on EC2

Basic steps:

```text
1. Create EC2 instance
2. Choose Ubuntu or Amazon Linux AMI
3. Select instance type
4. Create or select key pair
5. Configure security group
6. Allow SSH, HTTP, and HTTPS
7. Launch instance
8. Connect using SSH
9. Install web server
10. Upload website files
11. Access website using public IP or domain
```

Example Apache installation on Ubuntu:

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

Then open in browser:

```text
http://your-ec2-public-ip
```

---

## 27. Example: Deploying Angular App on EC2

Basic flow:

```text
Angular Project
   ↓
ng build
   ↓
dist folder
   ↓
Copy files to EC2
   ↓
Serve using Nginx
   ↓
Access using EC2 public IP
```

Install Nginx:

```bash
sudo apt update
sudo apt install nginx -y
```

Copy Angular build files to:

```text
/var/www/angular-app
```

Example Nginx configuration:

```nginx
server {
    listen 80;
    server_name your-ec2-public-ip;

    root /var/www/angular-app;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

---

## 28. Best Practices for EC2

Important best practices:

* Use IAM roles instead of storing access keys
* Allow only required ports in security groups
* Keep SSH access limited to your IP
* Use private subnets for databases
* Keep the operating system updated
* Take regular EBS snapshots
* Use Auto Scaling for production workloads
* Use load balancers for high availability
* Monitor instances using CloudWatch
* Stop unused instances to reduce cost
* Use tags to organize resources
* Avoid running everything on one instance
* Use Elastic IP only when necessary

---

## 29. Common Beginner Mistakes

| Mistake                                    | Problem                             |
| ------------------------------------------ | ----------------------------------- |
| Opening SSH to 0.0.0.0/0                   | Anyone can try to connect           |
| Forgetting to stop EC2                     | Cost increases                      |
| Losing private key                         | Cannot easily connect               |
| Putting database in public subnet          | Security risk                       |
| Not taking backups                         | Risk of data loss                   |
| Choosing large instance type unnecessarily | Higher cost                         |
| Terminating instead of stopping            | Instance may be permanently deleted |
| Not allowing port 80                       | Website cannot be accessed          |
| Wrong username for SSH                     | Login fails                         |

---

## 30. EC2 with Public and Private Subnets

Example architecture:

```text
AWS Cloud
└── VPC: 10.0.0.0/16
    ├── Public Subnet: 10.0.1.0/24
    │   ├── EC2 Web Server
    │   ├── Public IP
    │   └── Route: 0.0.0.0/0 → Internet Gateway
    │
    └── Private Subnet: 10.0.2.0/24
        ├── EC2 App Server
        ├── EC2 Database Server
        └── No direct internet access
```

---

## 31. EC2 Summary

Amazon EC2 is one of the most important AWS services. It provides virtual servers in the cloud that can be used to run websites, applications, databases, APIs, and other workloads.

Key things to remember:

```text
EC2 = Virtual server in AWS
AMI = Template for EC2
Instance type = CPU/RAM capacity
Key pair = Secure login
Security group = Firewall
EBS = Storage
VPC = Network
Subnet = Smaller network inside VPC
Elastic IP = Static public IP
Auto Scaling = Automatic increase/decrease of instances
Load Balancer = Distributes traffic
CloudWatch = Monitoring
```

---

## 32. Simple Final Explanation

In simple words:

> EC2 is a cloud-based virtual server. You choose an operating system, select the server size, attach storage, configure networking and security, and then use it to run applications or websites.

Example:

```text
User opens website
   ↓
Internet
   ↓
Internet Gateway
   ↓
Public Subnet
   ↓
EC2 Web Server
   ↓
Website is displayed
```
