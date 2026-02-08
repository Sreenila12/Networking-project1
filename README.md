🌐 AWS Networking Project – VPC, Subnets & Internet Gateway
📌 Project Overview
This project demonstrates how to design a basic AWS Virtual Private Cloud (VPC) from scratch and make it internet accessible using an Internet Gateway and public subnets.
The goal of this project is to understand core cloud networking concepts used in real production environments.

🧠 What We Built
We created:
1 Custom VPC
Multiple Subnets
Internet Gateway (IGW)
Route Table with Internet Access
This architecture forms the foundation of almost every cloud infrastructure.

🏗️ Architecture Diagram (Concept)
Internet
    │
Internet Gateway
    │
Route Table (0.0.0.0/0 → IGW)
    │
VPC (10.0.0.0/16)
 ├── Public Subnet 1 (10.0.1.0/24)
 └── Public Subnet 2 (10.0.2.0/24)

🌍 Step 1 — Creating the VPC
VPC CIDR Used: 10.0.0.0/16
Why did we choose 10.0.0.0/16 ?
This is the most important networking decision in the project.
Private IP Ranges (RFC1918)
Cloud networks must use private IP ranges that are not routable on the internet.
The allowed private ranges are:

Range	Size
10.0.0.0 – 10.255.255.255	Very Large
172.16.0.0 – 172.31.255.255	Medium
192.168.0.0 – 192.168.255.255	Small

We chose 10.0.0.0/16 because it gives huge scalability.
🔢 Understanding /16 in Networking Terms
CIDR block = 10.0.0.0/16

Binary concept:

10.0.0.0 = 00001010.00000000.00000000.00000000
/16 = first 16 bits are network bits

Network bits → first 2 octets (10.0)
Host bits → last 2 octets

Available IP addresses:
2
32
−
16
=
2
16
=
65,536
 IP addresses


👉 We now have 65K private IPs inside our VPC.

🎯 Why This Matters in Real Projects
Large companies always start with a big VPC CIDR because:
Future scaling (microservices, Kubernetes, DBs)
Avoid re-design later (changing VPC CIDR is hard!)
Support multiple environments inside same VPC

Example:

Dev environment

Test environment

Production environment

All can live inside the same VPC.

🧩 Step 2 — Creating Subnets

A VPC is one big network.
We divide it into smaller networks called subnets.

We created:

Subnet	CIDR	Purpose
Public Subnet 1	10.0.1.0/24	Web servers
Public Subnet 2	10.0.2.0/24	Load balancing / scaling
🔢 Why /24 Subnets?

/24 means:

2
32
−
24
=
256
 IP addresses per subnet
2
32−24
=256 IP addresses per subnet

Why this is ideal:

Enough IPs for servers
Easy to manage
Standard industry practice

🎯 Why Multiple Subnets?

Real production systems need:
High availability
Fault tolerance
Isolation between services

AWS uses Availability Zones (AZs).
Each subnet can be placed in a different AZ.

If one data center fails → system still runs.

This is called High Availability Architecture.

🌐 Step 3 — Internet Gateway

By default:
Resources inside a VPC cannot access the internet.
We created an Internet Gateway (IGW) and attached it to the VPC.

Role of Internet Gateway:
Allows traffic between VPC and Internet
Makes public subnets truly “public”

🛣️ Step 4 — Route Table Configuration

We created a Route Table and added this route:

Destination: 0.0.0.0/0
Target: Internet Gateway

What does 0.0.0.0/0 mean?

It means:

“Send ALL internet traffic to the Internet Gateway”

This enables:
Outbound internet access
SSH access to EC2
Web hosting

🔓 What Makes a Subnet PUBLIC?

A subnet becomes public when:
✔ Route table points to Internet Gateway
✔ Instances have public IPs

Without both → subnet stays private.

🚀 Real-World Use Cases

This setup is used for:
Hosting web applications
Deploying cloud servers
Kubernetes clusters
Load balancers
DevOps environments
This is the foundation of cloud infrastructure.
🎓 Key Networking Concepts Learned

CIDR & IP addressing

Private vs Public networking

Subnetting
Routing
Internet Gateway
High Availability design
