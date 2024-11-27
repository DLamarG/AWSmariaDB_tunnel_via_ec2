### AWSmariaDB_tunnel_via_ec2


## **Step 1: Create an EC2 Instance**

1. **Log in to AWS Management Console**:
    
    - Navigate to the EC2 dashboard.
2. **Launch an EC2 Instance**:
    
    - Click **Launch Instance**.
    - Choose an Amazon Machine Image (AMI), e.g., **Amazon Linux 2** or **Ubuntu**.
    - Select an appropriate instance type (e.g., `t2.micro` for free tier).
    - **Configure Instance Details**:
        - Ensure the instance is launched in the same VPC and subnet as the RDS instance.
    - **Add Storage**: Use the default storage size or increase as needed.
    - **Add Tags** (optional): Name the instance, e.g., `ec2-tunnel-instance`.
    - **Configure Security Group**:
        - Allow **SSH (port 22)** from your IP (or `0.0.0.0/0` temporarily for testing).
    - **Key Pair**:
        - Select an existing key pair or create a new one (download and save the `.pem` file).
3. **Launch the Instance**:
    
    - Click **Launch** and wait for it to start.
4. **Connect to the EC2 Instance**:
    
    - Use SSH to connect to the instance:
        
        bash
        
        Copy code
        
        `ssh -i your-key.pem ec2-user@<EC2-Public-IP>`



## **Step 2: Create a MariaDB RDS Instance**

1. **Go to the RDS Dashboard**:
    
    - Navigate to **Databases** and click **Create Database**.
2. **Select Database Creation Method**:
    
    - Choose **Standard Create**.
3. **Engine Options**:
    
    - Select **MariaDB**.
4. **Version**:
    
    - Choose the desired version (e.g., `10.6`).
5. **Instance Class**:
    
    - Choose a free-tier eligible class like `db.t2.micro`.
6. **Storage**:
    
    - Configure the storage size as needed (e.g., 20 GB).
7. **Database Settings**:
    
    - **DB Instance Identifier**: Name your instance (e.g., `mariadb-rds`).
    - **Master Username**: Choose a username (e.g., `admin`).
    - **Master Password**: Enter and confirm a password.
8. **Connectivity**:
    
    - Ensure the RDS instance is in the same VPC as your EC2 instance.
    - Select **Public Access: No** (for security).
    - **VPC Security Group**: Create or select a security group with the following rules:
        - **Inbound Rule**:
            - Allow port **3306** from the **EC2 Security Group**.
9. **Additional Configuration**:
    
    - Adjust backup, monitoring, and maintenance settings as needed.
10. **Create the Database**:
    
    - Click **Create Database** and wait for the RDS instance to become available.



## **Step 3: Configure Security Group for EC2 and RDS**

1. **Update the EC2 Security Group**:
    
    - Add an **outbound rule** to allow traffic on port **3306** to the RDS instance’s security group.
2. **Update the RDS Security Group**:
    
    - Add an **inbound rule** to allow traffic from the EC2 instance’s security group




Here’s a step-by-step guide for setting up an **EC2 instance**, **MariaDB RDS instance**, and connecting to the database via **DBeaver using an SSH tunnel**.

---

## **Step 1: Create an EC2 Instance**

1. **Log in to AWS Management Console**:
    
    - Navigate to the EC2 dashboard.
2. **Launch an EC2 Instance**:
    
    - Click **Launch Instance**.
    - Choose an Amazon Machine Image (AMI), e.g., **Amazon Linux 2** or **Ubuntu**.
    - Select an appropriate instance type (e.g., `t2.micro` for free tier).
    - **Configure Instance Details**:
        - Ensure the instance is launched in the same VPC and subnet as the RDS instance.
    - **Add Storage**: Use the default storage size or increase as needed.
    - **Add Tags** (optional): Name the instance, e.g., `ec2-tunnel-instance`.
    - **Configure Security Group**:
        - Allow **SSH (port 22)** from your IP (or `0.0.0.0/0` temporarily for testing).
    - **Key Pair**:
        - Select an existing key pair or create a new one (download and save the `.pem` file).
3. **Launch the Instance**:
    
    - Click **Launch** and wait for it to start.
4. **Connect to the EC2 Instance**:
    
    - Use SSH to connect to the instance:
        
        bash
        
        Copy code
        
        `ssh -i your-key.pem ec2-user@<EC2-Public-IP>`
        

---

## **Step 2: Create a MariaDB RDS Instance**

1. **Go to the RDS Dashboard**:
    
    - Navigate to **Databases** and click **Create Database**.
2. **Select Database Creation Method**:
    
    - Choose **Standard Create**.
3. **Engine Options**:
    
    - Select **MariaDB**.
4. **Version**:
    
    - Choose the desired version (e.g., `10.6`).
5. **Instance Class**:
    
    - Choose a free-tier eligible class like `db.t2.micro`.
6. **Storage**:
    
    - Configure the storage size as needed (e.g., 20 GB).
7. **Database Settings**:
    
    - **DB Instance Identifier**: Name your instance (e.g., `mariadb-rds`).
    - **Master Username**: Choose a username (e.g., `admin`).
    - **Master Password**: Enter and confirm a password.
8. **Connectivity**:
    
    - Ensure the RDS instance is in the same VPC as your EC2 instance.
    - Select **Public Access: No** (for security).
    - **VPC Security Group**: Create or select a security group with the following rules:
        - **Inbound Rule**:
            - Allow port **3306** from the **EC2 Security Group**.
9. **Additional Configuration**:
    
    - Adjust backup, monitoring, and maintenance settings as needed.
10. **Create the Database**:
    
    - Click **Create Database** and wait for the RDS instance to become available.

---

## **Step 3: Configure Security Group for EC2 and RDS**

1. **Update the EC2 Security Group**:
    
    - Add an **outbound rule** to allow traffic on port **3306** to the RDS instance’s security group.
2. **Update the RDS Security Group**:
    
    - Add an **inbound rule** to allow traffic from the EC2 instance’s security group.

---

## **Step 4: Test Connectivity from EC2 to RDS**

1. **SSH into EC2 Instance**:
    
    bash
    
    Copy code
    
    `ssh -i your-key.pem ec2-user@<EC2-Public-IP>`
    
2. **Install MariaDB Client** (if not already installed):
    
    - For Amazon Linux/Ubuntu:
        
        bash
        
        Copy code
        
        `sudo yum install mariadb -y # OR sudo apt update && sudo apt install mariadb-client -y`
        
3. **Connect to the RDS Database**:
    
    bash
    
    Copy code
    
    `mysql -h <RDS-Endpoint> -u <username> -p`
    
    - Replace `<RDS-Endpoint>` with the endpoint of your RDS instance (found in the AWS RDS dashboard).
    - Enter the password when prompted.





## **Step 5: Set Up DBeaver to Connect via SSH Tunnel**

1. **Download and Install DBeaver**:
    
    - Install DBeaver on your local machine if not already installed.
2. **Create a New Connection**:
    
    - Open DBeaver and click **Database > New Connection**.
    - Select **MariaDB** from the list and click **Next**.
3. **Connection Settings**:
    
    - **Host**: The **RDS Endpoint**.
    - **Port**: `3306`.
    - **Database**: Your RDS database name.
    - **Username**: The RDS username.
    - **Password**: The RDS password.
4. **Configure SSH Tunnel**:
    
    - Go to the **SSH** tab in the connection settings.
    - Check **Use SSH Tunnel**.
    - **Host**: The **Public IP** of your EC2 instance.
    - **Port**: `22`.
    - **Username**: `ec2-user` (or appropriate user for your AMI).
    - **Private Key**: Browse and select your `.pem` file.
5. **Test Connection**:
    
    - Click **Test Connection** to verify everything is working.
    - If successful, save the connection.


## **Step 6: Access the Database**

- Once connected, you can use DBeaver to query and manage your MariaDB database securely through the SSH tunnel.
