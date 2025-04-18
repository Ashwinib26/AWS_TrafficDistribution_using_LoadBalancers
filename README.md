# AWS_TrafficDistribution_using_LoadBalancers

Here's a **step-by-step guide** for your assignment to deploy two EC2 instances with a dummy web page and attach them to a Load Balancer.

---


### **Step 1: Launch First EC2 Instance**
1. Go to **EC2 Dashboard** → **Launch Instance**
2. **Name:** `Instance-1`
3. **AMI:** Choose Amazon Linux 2 AMI
4. **Instance Type:** `t2.micro` (free tier)
5. **Key Pair:** Select or create a new one
6. **Network Settings:**
   - Allow **HTTP** (port 80) and **SSH** (port 22)
7. **Advanced Details → User data:**
   Paste this script:
   ```bash
   #!/bin/bash
   yum update -y
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
   ```
8. Click **Launch Instance**

---

### **Step 2: Launch Second EC2 Instance**
Repeat **Step 2**, but name this one `Instance-2`.

---

### **Step 3: Create a Target Group**
1. Go to **EC2 → Load Balancers → Target Groups**
2. Click **Create target group**
   - **Target type:** Instances
   - **Protocol:** HTTP
   - **Port:** 80
   - **VPC:** Choose the same as your EC2 instances
3. Name it: `MyTargetGroup`
4. Click **Next**, and **Register both EC2 instances** to this target group
5. Click **Create target group**

---

### **Step 4: Create a Load Balancer**
1. Go to **EC2 → Load Balancers**
2. Click **Create Load Balancer → Application Load Balancer**
3. **Name:** `MyALB`
4. **Scheme:** Internet-facing
5. **Listeners:** HTTP (Port 80)
6. **Availability Zones:** Select at least 2 and their subnets
7. **Security Group:**
   - Allow **HTTP (port 80)**
8. **Target Group:**
   - Select `MyTargetGroup` (created in Step 4)
9. Click **Create Load Balancer**

---

### **Step 5: Test the Load Balancer**
1. Go to **EC2 → Load Balancers**
2. Copy the **DNS name** of your load balancer (e.g., `myalb-123456789.us-east-1.elb.amazonaws.com`)
3. Paste it into your browser.
4. **Refresh the page multiple times**, you should see:
   ```
   Hello World from ip-172-31-XX-XX.ec2.internal
   ```
   - The message will change as it hits different instances.

---
