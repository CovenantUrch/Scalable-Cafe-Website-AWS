<h1>☕️ Scalable and Highly Available Café Website on AWS</h1>

This project demonstrates how to design and deploy a **highly available, scalable, and resilient web application infrastructure** on **Amazon Web Services (AWS)**. The goal is to ensure that the café’s website can handle sudden traffic spikes, such as when featured on a major TV food show.

---

<h2>📌 Project Overview</h2>

**Objective:**  
- Build a web server architecture that automatically scales in and out based on demand.  
- Distribute incoming traffic across multiple servers to ensure zero downtime and great user experience.  
- Deploy across multiple Availability Zones (AZs) for fault tolerance.

**AWS Services Used:**  
- Virtual Private Cloud (VPC)  
- Subnets (Public and Private, across multiple AZs)  
- Internet Gateway (IGW)  
- Route Tables  
- Elastic Load Balancing (Application Load Balancer)  
- EC2 Launch Template  
- EC2 Auto Scaling Group (ASG)  
- CloudWatch Alarms and Metrics

---

At the end of this project, your architecture should look like the following example:
<br/>
<img src="db 1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

---

## <h2>⚙️ Steps to Deploy</h2>

1️⃣ Create a Virtual Private Cloud (VPC)
- **CIDR Block:** `10.0.0.0/16`
- Enable **DNS Hostnames**

2️⃣ Configure Subnets & Networking
- **Public Subnets:** For ALB (e.g., `10.0.1.0/24` and `10.0.2.0/24`)
- **Private Subnets:** For EC2 instances (e.g., `10.0.3.0/24` and `10.0.4.0/24`)
- Attach an **Internet Gateway (IGW)**
- Update **Route Tables**:
  - Public subnets route through IGW
  - Private subnets stay private

3️⃣ Create a Launch Template
- **AMI:** Amazon Linux 2023 or custom café app image
- **Instance Type:** `t2.micro` (or as needed)
- **Key Pair:** Choose Create new key pair.
Tip: Create a new key pair and choose it. Make sure that you download the key pair to your local computer. For SSH access
- **Network Settings:** Auto-assign public IP = disabled
- **Security Group:** Allow HTTP (80) and SSH (22) from trusted IPs
- Optional **User Data Script** to bootstrap the café app

4️⃣ Set Up Auto Scaling Group (ASG)
- Launch template: Use the created one
- Attach to VPC private subnets in multiple AZs
- Register with ALB target group
- Desired Capacity: 2 instances
- Minimum Capacity: 2
- Maximum Capacity: 6
- Automatic scaling - optional: Choose Target tracking scaling policy, and configure the following options
- Metric type: Choose Average CPU utilization.
- Target value: Enter 25
- Instance warmup: Enter 60

5️⃣ Deploy an Application Load Balancer (ALB)
- Type: **Application Load Balancer**
- Scheme: **Internet-facing**
- Subnets: Use public subnets in 2 AZs.
- **HTTPS security configuration settings**: Skip these settings.
- **Security groups**: Create a new security group that allows HTTP traffic from anywhere
- **Target group**: Create a new target group.
- **Registering targets**: Skip this section.
- Listener: HTTP on port 80

Modify the Auto Scaling group that you created in the previous task by adding this new load balancer.
Hint: Add the target group that you created in the Application Load Balancer configuration.

6️⃣ Test the Setup
- Obtain the **ALB DNS name** and open in a browser.
- Verify traffic is balanced between servers.

<h2>Testing automatic scaling under load</h2>

By using AWS Systems Manager Session Manager, connect to one of the running web server instances.
On the sessions manager terminal, use the following commands to start a stress test. This test increases the load on the web server CPU:
<br/>
<img src="db 2.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

---

Verify that the Auto Scaling group deploys new instances.
Continue to observe the Amazon EC2 console.
During the test, you should observe that more web server instances are deployed.

✅ Outcomes
- By following this project, you achieve:
- Automatic scalability to handle peak traffic.
- High availability across multiple AZs.
- Load balancing for optimal performance.
- Resilient architecture for real-world production scenarios.

<h1>📁 Author</h1>
Project by: Covenant Onwukwe
Contact: covenanturch@gmail.com
