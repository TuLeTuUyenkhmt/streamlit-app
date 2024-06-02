**Air Quality Forecasting of CO2 Emission (Global)**

Objective : 
- To forecast CO2 levels for an organization so that the organization can follow government norms with respect to CO2 emission levels.
- Given data of CO2 emission of the world from 1800 to 2014. The aim is to study the data and predict the CO2 emission for next 10 years. 

Work Flow :
- Data Analysis and drawing up some inferences. 
- Implementation of various EDA techniques to clean the data and visualized it using different graphs.
- Future prediction had to be done using 'Time Series Analysis & Forecasting' technique. 
- Model Deployement using Streamlit API and Streamlit Cloud.

####Guide to deploy Streamlit app in AWS

### Deploying the Web App on AWS: Step-by-Step

#### Step 1: Set up the VPC
1. **Create VPC**:
   - Name: MyVPC
   - CIDR: 10.0.0.0/16

2. **Create Subnet**:
   - Name: PublicSubnet
   - CIDR: 10.0.1.0/24
   - Availability Zone: us-east-1a

3. **Create Internet Gateway**:
   - Name: MyInternetGateway
   - Attach to MyVPC

4. **Create Route Table**:
   - Name: PublicRouteTable
   - Add Route: 0.0.0.0/0 -> MyInternetGateway
   - Associate with PublicSubnet

#### Step 2: Launch an EC2 Instance
1. **Launch EC2 Instance**:
   - AMI: Amazon Linux 2
   - Instance Type: t2.micro
   - Key Pair: Create or use existing
   - Network: MyVPC
   - Subnet: PublicSubnet
   - Auto-assign Public IP: Enable

2. **Configure Security Group**:
   - Create Security Group:
     - SSH (port 22): Source My IP
     - HTTP (port 8501): Source Anywhere

3. **Connect to EC2**:
   ```sh
   ssh -i "your-key-pair.pem" ec2-user@your-instance-public-dns
   ```

4. **Install Required Software**:
   ```sh
   sudo yum update -y
   sudo yum install python3 -y
   pip3 install --upgrade pip
   pip3 install streamlit pandas openpyxl plotly
   ```

#### Step 3: Set up S3 for Data Storage
1. **Create S3 Bucket**:
   - Name: my-co2-bucket
   - Region: us-east-1

2. **Upload Dataset to S3**:
   - Upload CO2 dataset.xlsx to my-co2-bucket

#### Step 4: Configure IAM
1. **Create IAM Role**:
   - Service: EC2
   - Policy: AmazonS3ReadOnlyAccess

2. **Attach IAM Role to EC2 Instance**:
   - Go to EC2 Dashboard
   - Select the instance
   - Actions -> Security -> Modify IAM Role
   - Select the created role

#### Step 5: Deploy Streamlit Application
1. **Copy and Create `app.py` on EC2**:
   - Create `app.py` file with your Streamlit code

2. **Run Streamlit Application**:
   ```sh
   streamlit run app.py --server.port 8501
   ```

3. **Access the Application**:
   - Open a web browser and navigate to `http://your-instance-public-dns:8501`

