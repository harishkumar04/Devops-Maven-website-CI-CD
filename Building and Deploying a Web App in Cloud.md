# Build and Deply a Web App in Cloud

# Introducing the project

Iam going to build a CI/CD pipeline and deploy a web app that was developed from Scratch and deploy it on a EC2 instance.This project is part one of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project in the upcoming days where I will connecting my Github repo to AWS.

# Key tools and concepts
Services I used were AWS EC2, Apache Maven, SSH, Vs code. Key concepts I learnt include how to create and launch an EC2 instance, how to create a web app using Maven and finally how to connect my VS code to the EC2 instance using Remote SSH extension.

# Project Reflection
This project took me approximately two and half an hour to complete. The most challenging part was the ssh and maven coding part. It was most rewarding to see my web app files in the EC2 instance via VS code and being able to edit it.

# Project steps

## Setup the IAM user account

An IAM (Identity and Access Management) account in AWS is a user identity created within your AWS account to manage access securely without using the root user. Unlike the **root user**, which has unrestricted access to all AWS resources, IAM users have only the permissions explicitly granted to them. It's a best practice to use IAM accounts for daily operations and reserve the root account for critical administrative tasks.

## Launching an EC2 Instance

Before launching the EC2 instance it is also important to choose the Region to avail the services properly.

### *1. Create a name for the web server* ###

<img width="688" alt="Screenshot 2025-05-23 at 5 52 24 PM" src="https://github.com/user-attachments/assets/d7b5fe9f-abec-4ff6-bf9f-4365d51e8129" />

### *2. Choose the AMI (Amazon Machine Image) and the Instance Type* ###

- Here, Iam choosing "Amazon Linux 2023 AMI" since it is eligible for the free tier that means we don't have to pay anything to run this instance.
  
- The Instance type is "t2.micro" again since it is elgible for the free tier

- The Difference between AMI and Instance type is that AMI is more related to the OS side whereas the Instance type is Hardware related. Let's take the analogy of baking a cake,

  - AMI tells us about the key ingredients and whereas the Instance type tells us about the Cake size.

<img width="909" alt="Screenshot 2025-05-23 at 5 59 59 PM" src="https://github.com/user-attachments/assets/d2a75145-e5d9-4f96-9768-224071974cfc" />

### *3. Key Pair* ###

Key pair is used to connect to your instance on the cloud. A key pair in EC2 is like the keys to your virtual computer. Just like you need a key to unlock and start your car, a key pair lets you securely access your EC2 instance.

It's made of two halves: a public key that AWS keeps, and a private key that you download.

When you use the private key, it verifies that you're the one allowed to access that specific virtual machine, keeping everything secure and just for you. It uses RSA to generate the key pair or u can choose among the other algorithms that are avaliable to generate your key pair.

#### *3.1 Download the key as a Pem File* ####

Pem stands for Privacy Enhanced Mail that we need to connect to our EC2 instance in the cloud. Don't lose this pem file since you can't recover this file meaning once it is lost you have to create another new key pair and then repeat the process again.

## That is it, now we can launch the EC2 instance

## Connect to the EC2 instance using the Terminal

Now, we will connect to our EC2 instance inside which we will create a web app. A Terminal is place which we can use to send commands to the computer. The first command I ran for this project is cd Desktop/Devops to navigate to the Devops folder that I created. I also updated my Private key's permissions by using the command 'chmod'. 

```git
chmod 400 [filename.pem]
```
Here, chmod stands for 'Change Mode' and it sets the file as readable to owner and no permissions for group and others

### *1. Use SSH to connect to the terminal to the EC2 instance* ###

SSH stands for Secure Shell is a protocol used to make sure only authorized users can access a remote server. When you connect to your EC2 instance later in this project, SSH verifies you have the correct private key that matches the public key on the server.


SSH is also a type of network traffic. Once SSH has authorized you, it'll set up a secure connection between you and the EC2 instance. All data transferred (including your commands and the responses from the instance) gets encrypted. This encryption makes SSH an ideal method for working with virtual servers.

Before, connecting to the EC2 instance we need the Public Ip address of the EC2 instance otherwise the system don't know where is our EC2 instance and which one to connect to. So, copy that address which will avaliable once you click on the details of the instance.

<img width="265" alt="Screenshot 2025-05-23 at 9 43 26 PM" src="https://github.com/user-attachments/assets/9e9224f1-7513-48b9-b882-edbef0c6c617" />

Now, to connect to the EC2 instance use this command 

```git
ssh -i [Path name to the private key] ec2-user@[Public DNS server address]
```

Type 'yes' in the terminal to connect.

We can see the change in the name of the root user in the terminal

<img width="504" alt="Screenshot 2025-05-24 at 9 30 34 PM" src="https://github.com/user-attachments/assets/a1628b4f-d9c2-4975-af9e-858c69d1bc43" />

## *Creating the Web app*

*Apache Maven*  is a tool that helps developers build and organize Java software projects. It's also a package manager, which means it automatically download any external pieces of code your project depends on to work.

It uses something called archetypes, which are like templates, to lay out the foundations for different types of projects e.g. web apps.

Java is required in this project because we are using Apache Maven which needs
Java to write the code for our web app.

Now, we have to install Maven in the EC2 instance which can be done using this commands below

```bash
wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
sudo tar -xzf apache-maven-3.5.2-bin.tar.gz -C /opt
echo "export PATH=/opt/apache-maven-3.5.2/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```



            

