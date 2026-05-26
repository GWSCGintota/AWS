
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jhgejv11j4sbgglh2mfr.png)

## **1.1 What is the VPC**
In simple words, a **VPC is your own private network inside AWS cloud**. You can place your servers, databases, and other cloud resources inside it and control how they communicate with the internet and with each other.

Think of it like this:

**AWS Cloud = a big city
Your VPC = your own private land/area inside that city**
Inside that area, you decide:

- which servers are public
- which servers are private
- who can access them
- how traffic goes in and out
- what security rules are applied

In your diagram:

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qux3tz84fnjajdtzeqn3.png)
Example:

A user from the internet sends a request to your website.
That request goes through the **Internet Gateway** and reaches the Web Server in the Public Subnet.
Then the web server can communicate with the Database in the Private Subnet.
But the database is not directly open to the internet, so it is more secure.

So, the main purpose of a VPC is to **securely organize and control your cloud network**.


## 1.2 Core Component 

1. **Subnet**

A subnet in a VPC is a smaller section of the VPC’s IP address range. There are three type of subnets

```plaintext
1. Public Subnet
2. Private Subnet
3. Isolated Subnet
```

Think of a VPC as your private network in AWS, and subnets as smaller rooms/areas inside that network.

Example:

```plaintext
VPC CIDR: 10.0.0.0/16

Public Subnet:  10.0.1.0/24
Private Subnet: 10.0.2.0/24

```

Why do we use subnets?

Subnets help you separate resources based on access level.

**Public subnet**

A public subnet is connected to the internet through an Internet Gateway.

Example resources:


- Web server
- EC2 instance that needs internet access
- Load balancer

**Private subnet**

A private subnet is not directly accessible from the internet.

Example resources:

- Database server
- Application backend
- Internal services

Simple example

```plaintext
VPC
 ├── Public Subnet
 │    └── EC2 Web Server
 │         Accessible from internet using HTTP/HTTPS/SSH
 │
 └── Private Subnet
      └── Database Server
           Not directly accessible from internet
```

So, in simple words:

A subnet is a smaller network inside a VPC where you place AWS resources like EC2 servers, databases, and load balancers.

2. Internet Gateway (IGW)

**An Internet Gateway (IGW)** in Amazon Web Services is a component that allows your VPC (Virtual Private Cloud) to communicate with the internet.

Think of it like a door between your AWS network and the public internet.

Simple Explanation
Without an Internet Gateway:

```plaintext
EC2 Instance ❌ Internet
```
With an Internet Gateway:

```plaintext
EC2 Instance ↔ Internet Gateway ↔ Internet
```
**What It does**
An Internet Gateway allows:

- Incoming internet traffic to your AWS resources
- Outgoing internet access from your instances

**3.Router Table**
A Route Table in Amazon Web Services is a set of rules that tells your VPC where network traffic should go.

Think of it like a GPS or traffic controller for your AWS network.

**Simple Explanation**
When data leaves an EC2 instance, the route table decides:

```plaintext
"Where should I send this traffic?"
```
Examples:

- To the internet
- To another subnet
- To a VPN
- To another VPC

 

## Build a Custom VPC from Scratch

Create

1. VPC
2. Public Subnet
3. Private Subnet
4. Internet Gateway
5. Route Tables

**STEP 01 -  Create the VPC**
1. In the AWS Console search bar, type **VPC** and click **VPC** under Services.
2. In the left sidebar, click **Your VPCs**.
3. Click the orange **Create VPC** button (top right).
4. Fill in the form:
   - **Resources to create:** Select `VPC only` (not "VPC and more" -- we will do it manually for learning)
   - **Name tag:** `my-training-vpc`
   - **IPv4 CIDR block:** `10.0.0.0/16`
   - **IPv6 CIDR block:** `No IPv6 CIDR block`
   - **Tenancy:** `Default`
5. Click **Create VPC**.
6. You should see a green success banner. Note the **VPC ID** (e.g., `vpc-0abc1234...`).



STEP 2 - Create the Public Subnet

1. In the left sidebar, click **Subnets**.
2. Click **Create subnet**.
3. Fill in:
   - **VPC ID:** Select `my-training-vpc` from the dropdown
4. Under **Subnet settings**:
   - **Subnet name:** `my-public-subnet`
   - **Availability Zone:** Choose the first AZ in the list (e.g., `ap-south-1a`)
   - **IPv4 CIDR block:** `10.0.1.0/24`
5. Click **Create subnet**.
6. Subnet created. Note the **Subnet ID**.



**STEP 3 - Create the Private Subnet**


1. Click **Create subnet** again.
2. Fill in:
   - **VPC ID:** Select `my-training-vpc`
3. Under **Subnet settings**:
   - **Subnet name:** `my-private-subnet`
   - **Availability Zone:** You can use the same AZ or a different one (e.g., `ap-south-1b`)
   - **IPv4 CIDR block:** `10.0.2.0/24`
4. Click **Create subnet**.
5. Private subnet created.

**STEP 4 - Create and Attach an Internet Gateway**

1. In the left sidebar, click **Internet Gateways**.
2. Click **Create internet gateway**.
3. Fill in:
   - **Name tag:** `my-training-igw`
4. Click **Create internet gateway**.
5. You will see the IGW is created but its **State** shows `Detached`.
6. **Now attach it to your VPC:**
   - With the new IGW selected, click the **Actions** button (top right).
   - Click **Attach to VPC**.
   - In the **Available VPCs** dropdown, select `my-training-vpc`.
   - Click **Attach internet gateway**.
7. The IGW State should now show `Attached`.

**STEP 5 - Create a Public Route Table**
AWS creates a **Main route table** for every VPC automatically. Best practice is **not to modify** the main route table (it applies to all subnets by default). Instead, we create a dedicated one for our public subnet.

1. In the left sidebar, click **Route Tables**.
2. You will see an existing route table -- this is the **main** one for `my-training-vpc`. Notice it only has the local route (`10.0.0.0/16 -> local`).
3. Click **Create route table**.
4. Fill in:
   - **Name:** `my-public-route-table`
   - **VPC:** Select `my-training-vpc`
5. Click **Create route table**.
6. New route table created.

**STEP 6 - Add the Internet Route to the Public Route Table**

1. Click on `my-public-route-table` to open its details.
2. Click the **Routes** tab.
3. Click **Edit routes**.
4. Click **Add route**.
5. Fill in the new route:
   - **Destination:** `0.0.0.0/0` -- This means "all traffic going anywhere on the internet"
   - **Target:** Click the dropdown, select **Internet Gateway**, then select `my-training-igw`
6. Click **Save changes**.
7. The Routes tab should now show two routes:
   - `10.0.0.0/16 -> local` (VPC internal traffic)
   - `0.0.0.0/0 -> igw-xxxxxxxx` (internet traffic)

**STEP 7 - Associate the Public Subnet with the Public Route Table**

Adding a route to the route table is not enough -- we need to explicitly tell the public subnet to use this route table.

1. Still on the `my-public-route-table` details page.
2. Click the **Subnet associations** tab.
3. Click **Edit subnet associations**.
4. Check the box next to `my-public-subnet`.
5. Click **Save associations**.
6. The public subnet is now associated with the public route table.

**STEP 8 - Enable Auto-assign Public IP for the Public Subnet**
When you launch an EC2 instance into the public subnet, it needs a public IP to be reachable from the internet.

1. In the left sidebar, click **Subnets**.
2. Select `my-public-subnet`.
3. Click **Actions -> Edit subnet settings**.
4. Under **Auto-assign IP settings**, check **Enable auto-assign public IPv4 address**.
5. Click **Save**.
6. Done. Any EC2 instance launched into this subnet will automatically receive a public IP.


**VPC Summary - What We Built**

```
VPC: my-training-vpc (10.0.0.0/16)
|
|-- Public Subnet: my-public-subnet (10.0.1.0/24)
|       |
|       +-- Route Table: my-public-route-table
|               |-- 10.0.0.0/16 -> local
|               +-- 0.0.0.0/0  -> my-training-igw   [Internet access]
|
|-- Private Subnet: my-private-subnet (10.0.2.0/24)
|       |
|       +-- Route Table: Main (auto-created)
|               +-- 10.0.0.0/16 -> local             [No internet access]
|
+-- Internet Gateway: my-training-igw (Attached)
```


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w08u8g1ej353ox9giykr.png)
**Key Takeaways - Networking**

- A **VPC** is your private network inside AWS
- **Subnets** are segments of that network, each scoped to one Availability Zone
- A subnet becomes **public** only when it has a route to an Internet Gateway
- **Route tables** are the traffic directors -- each subnet has one
- The **Internet Gateway** is the single entry and exit point for internet traffic
- Best practice: databases go in **private subnets**, web servers go in **public subnets**

