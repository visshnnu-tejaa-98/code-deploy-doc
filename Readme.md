# Activity 18

Deploy a simple Nginx application using AWS code commit and deploy & access via browser

Repository used: https://github.com/visshnnu-tejaa-98/code-deploy-repo

## Step 1: Create an EC2 Instance

1. Go to instance dashboard > click on **Launch instance**
2. Give Instance name
3. Select Key pair
4. Select security Group
5. Select instance type as **t2.micro**
6. click on **launch instance** button

   ![alt text](/images/instance.png)

## Step 2: Create a role for code deploy

1. Go to IAM Dashboard > click on create **IAM Role** button
2. Select usecase as codedeploy
3. click on **next** > **next** > **create role** button
4. Here you need to give the following policies

   - AmazonEC2FullAccess
   - AWSCodeDeployRole

   ![alt text](/images/iam-codedeploy.png)

## Step 3: Create a role for ec2

1. Go to IAM Dashboard > click on create **IAM Role** button
2. Select usecase as ec2
3. click on **next** button
4. select the following policies
   - AmazonS3FullAccess
   - AWSCodeDeployFullAccess
5. Click on **next** button
6. Click on **create role** button

   ![alt text](/images/iam-ec2.png)

## Step 4: Install code deploy agent in ec2 instance

1. You can install the code deploy agent in ec2 from here or follow the below steps: https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html

   ```bash
   # Updating Ubuntu
   sudo apt update

   # Installing ruby-full
   sudo apt install ruby-full

   # Installing wget
   sudo apt install wget

   # Change directory to ubutu
   cd /home/ubuntu
   ```

   ```bash
   # Connecting to S3 bucket
   # here we need to change the bucket name and region based on our environment
   wget https://<bucket-name>.s3.<region-identifier>.amazonaws.com/latest/install
   ```

   You can get the bucket name as per your region from here: https://docs.aws.amazon.com/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names

   After modifing the above command

   ```bash
   wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
   ```

## Step 5: Deployment application creation

1. Go to code deploy dashboard
2. Click on **Create Application** button
3. Give Application name
4. Select compute platform as EC2
5. Click on **Create Application** button

   ![alt text](/images/application.png)

## Step 6: Deployement group creation

1. Go to code deploy dashboard
2. Open newly created Application
3. Cick on **Create Deployement Group** button
4. Give Deployement group name (my-dg-1)
5. Select service role as **codedeploy-ec2** IAM role
6. Select **Amazon EC2 instances** in Environment Configuration section
7. Select our newly created EC2 instance for tags section
8. Deselect Load balancer in Load balancer section
9. Click on **Create Deployment Group** button
10. Now, you are able to see deployment group in the application section

![alt text](/images/deployment-grp.png)

## Step 7: Deployment creation

1. Go to code deploy dashboard
2. Open newly created Application
3. Open a newly created deployment group
4. Click on **Create Deployment** Button
5. Select **my application is stored in github** in Deployment settings section
6. Give Github token name (create a new if not present)
7. Give repository name as your github repository URL **(visshnnu-tejaa-98/code-deploy-repo)**
8. Give the latest commit id from your repository (c4754b66dec33b61bc0e059f422e2a1f2f361e16)
9. Click on **create Deployment** button
10. Now you will be able to see the Deployements lifecycle in lifecycle events section

    ![alt text](/images/dep-success.png)

## Step8: Pipeline Creation

1. Go to pipeline dashboard
2. Click on **Create Pipeline** button
3. Give Pipeline name
4. Click on **next** button
5. Select source provdider as **Github(Version 1)**
6. Click on **Connect to github** button
7. Specify the repository and branch
8. Click on **Skip Build Stahe** button
9. Give Deploy provider as **AWS codebuild**
10. Specify Application Name and deployement group
11. Click on **Create Pipeline** button
12. Wait for sometime, The pipline will be completed
13. Now copy your public ip for your instance and search in browser, you will able to see the web page running on nginx

    ![alt text](/images/web-page.png)
