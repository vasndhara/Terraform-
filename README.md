RUN A CICD PIPELINE FOR TERRAFORM TO INIT,FORMAT,VALIDATE,PLAN AND APPLY

JENKINS INSTALLATION 

First install Jenkins in a server 

Create a ec2 instance and connect to that instance then switch to root user by using 

(sudo su –) command.

install Jenkins using this commands 

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \ 

 hƩps://pkg.jenkins.io/debian/jenkins.io-2023.key 
 
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \ 

 hƩps://pkg.jenkins.io/debian binary/ | sudo tee \ 
 
 /etc/apt/sources.list.d/jenkins.list > /dev/null 
 
sudo apt-get update 

sudo apt-get install Jenkins 

Jenkins requires java to run2.INSTALL JAVA 

apt install openjdk-17-jdk -y : This command used to install java 

java -version : To check the java version 

aŌer installing Jenkins and java in the server we need to restart the Jenkins server 


systemctl restart Jenkins : to restart the Jenkins 

systemctl status Jenkins : to check the Jenkins status , whether its running or not 

3. LOGIN INTO JENKINS DASHBOARD
   
   
4.To access Jenkins. We need to allow inbound traffic on PORT 8080 in the AWS security

group. 
Copy the public ip of the server and paste in the browser with port number 8080{Example : 

54.89.243.242:8080} 

To unlock Jenkins we need administrator password 

Go to your terminal and paste the command below to get the password to unlock Jenkins 

cat /var/lib/jenkins/secrets/iniƟalAdminPassword

Select install suggested plugins 

Create an admin user by filling in the required informaƟon

Click save and finish. 

Now Jenkins setup has been completed. 

in Jenkins dashboard select manage Jenkins and click on plugins and install terraform 

Select install suggested plugins 

Create an admin user by filling in the required informaƟon

Click save and finish. 

Now Jenkins setup has been completed. 

in Jenkins dashboard select manage Jenkins and click on plugins and install terraform 

plugins. 

Now, again select manage Jenkins and click on tools and install terraform and save. 

Select credenƟals and add aws credenƟals and git credenƟals.

 
Create a vpc ,public and private subnet , internet gateway ,rout table and create a ec2 

instance in public subnet by using terraform. 

In Main.tf

![terraform-1](https://github.com/user-attachments/assets/5bf7d625-b9bd-4d9d-a9f4-8e98346adb36)

![terraform-2](https://github.com/user-attachments/assets/3508539b-68f5-4c71-a37b-8fb062023c44)

In variable.tf

![terraform-3](https://github.com/user-attachments/assets/9a467ce9-60a6-49f9-af0a-8fe8757a29f4)

In output.tf

![terraform-4](https://github.com/user-attachments/assets/5260cd92-b24f-4557-a4df-91d1649297af)

Create a backend.tf file to store state file in s3 bucket 

![terraform-5](https://github.com/user-attachments/assets/d41d2ee7-8c28-4d39-8609-23667bea2a89)

Create a Jenkins script for terraform to run a pipeline 

![terraform-6](https://github.com/user-attachments/assets/6fd9b589-8f9b-4fb3-9b35-3210f5026b5f)

![terraform-7](https://github.com/user-attachments/assets/3cb6a7a9-eb41-4a09-b66e-a6f17782d3b4)

Explanation 

pipeline: Defines a Jenkins pipeline.

agent any: Runs the pipeline on any available agent. 

environment: Sets environment variables.

AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY: Fetches AWS credentials stored in 

Jenkins. 

stage('Checkout'): Checks out the Terraform code from the specified Git repository.

stage('Terraform Init'): Initializes the Terraform configuration.

stage('Terraform Format'): Checks if the Terraform files are formatted correctly using 

terraform fmt -check. 

stage('Terraform Validate'): Validates the Terraform configuration using terraform validate.

stage('Terraform Plan'): Creates an execution plan using terraform plan and saves it to a file 

named tfplan.

stage('Terraform Apply'): Applies the changes required to reach the desired state of the 

configuration using the previously created plan (tfplan).

After creating all files push them into git repository. 

git init 

git remote add origin https://github.com/username/repository.git

git add . # Add all fils 

git commit -m "Commit message" 

git push -u origin master

use this commands to add your local files into git repository 

open the Jenkins dashboard and select the new item.

Enter an item name: terraform_cicd 

Select an item type: pipeline 

After selecting pipline click on ok .It opens Configuration in that select pipeline 

Select pipeline script from SCM in the definition 

SCM: Git 

Repository URL:git@github.com:vasndhara/terraform_cicd.git

Credentials : give your Credential 

Branch Specifier: main ( branch name where the files are stored )

Script Path: jenkinsfile (path name where the script is written)

After filling all this then click on apply and save it. 

Now select build now and it will run cicd pipeline.

This setup will provision a VPC with public and private subnets, an internet gateway, route

tables, and an EC2 instance in the public subnet using Terraform. The Jenkins pipeline will 

automate the Terraform workflow from initialization to applying changes.











   




