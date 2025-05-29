# Introducing the project

I am building a CI/CD pipeline to deploy a web application developed from scratch on an EC2 instance. This project is the first in a series of DevOps projects focused on CI/CD implementation using AWS. In the upcoming days, I’ll be working on the next phase, which involves connecting my GitHub repository to AWS.

# Key tools and concepts
Services used were AWS EC2, Apache Maven, SSH, Vs code. Key concepts that I learnt include how to create and launch an EC2 instance, how to create a web app using Maven and finally how to connect my VS code to the EC2 instance using Remote SSH extension.

# Project Reflection
The most challenging part was the ssh and maven coding part. It was most rewarding to see my web app files in the EC2 instance via VS code and being able to edit it.

# Project steps

## Setup the IAM user account

An IAM (Identity and Access Management) account in AWS is a user identity created within your AWS account to manage access securely without using the root user. Unlike the **root user**, which has unrestricted access to all AWS resources, IAM users have only the permissions explicitly granted to them. It's a best practice to use IAM accounts for daily operations and reserve the root account for critical administrative tasks.

## Launching an EC2 Instance

Before launching the EC2 instance, it is also important to choose the Region to avail the services properly.

### *1. Create a name for the web server* ###

<img width="688" alt="Screenshot 2025-05-23 at 5 52 24 PM" src="https://github.com/user-attachments/assets/d7b5fe9f-abec-4ff6-bf9f-4365d51e8129" />

### *2. Choose the AMI (Amazon Machine Image) and the Instance Type* ###

- Here, I am choosing "Amazon Linux 2023 AMI" since it is eligible for the free tier that means we don't have to pay anything to run this instance.
  
- The Instance type is "t2.micro" again since it is elgible for the free tier

- The Difference between AMI and Instance type is that AMI is more related to the OS side whereas the Instance type is Hardware related. Let's take the analogy of baking a cake,

  - AMI tells us about the key ingredients and whereas the Instance type tells us about the Cake size.

<img width="909" alt="Screenshot 2025-05-23 at 5 59 59 PM" src="https://github.com/user-attachments/assets/d2a75145-e5d9-4f96-9768-224071974cfc" />

### *3. Key Pair* ###

Key pair is used to connect to your instance on the cloud. A key pair in EC2 is like the keys to your virtual computer. Just like you need a key to unlock and start your car, a key pair lets you securely access your EC2 instance.

It's made of two halves: a public key that AWS keeps, and a private key that you download.

When you use the private key, it verifies that if you're the one allowed to access that specific virtual machine, keeping everything secure and just for you. It uses RSA to generate the key pair or you can choose among the other algorithms that are available to generate your key pair.

#### *3.1 Download the key as a Pem File* ####

PEM stands for Privacy Enhanced Mail that we need to connect to our EC2 instance in the cloud. Don't lose this pem file since you can't recover this file, as in once it is lost, you have to create another new key pair and then repeat the process again.

## Launching EC2 instance

## Connect to the EC2 instance using the Terminal

Now, we will connect to our EC2 instance inside which we will create a web app. A Terminal is place which we can use to send commands to the computer. The first command I ran for this project is cd Desktop/Devops to navigate to the Devops folder that I created. I also updated my Private key's permissions by using the command 'chmod'. 

```bash
chmod 400 [filename.pem]
```
Here, chmod stands for 'Change Mode' and it sets the file as readable to owner and no permissions for group and others

### *1. Use SSH to connect to the terminal to the EC2 instance* ###

SSH stands for Secure Shell, is a protocol used to make sure that only authorized users can access a remote server. When you connect to your EC2 instance later in this project, SSH verifies whether you have the correct private key that matches the public key on the server.

SSH is also a type of network traffic. Once SSH has authorized you, it'll set up a secure connection between you and the EC2 instance. All data transferred (including your commands and the responses from the instance) gets encrypted. This encryption makes SSH an ideal method for working with virtual servers.

Before, connecting to the EC2 instance we need the Public IP address of the EC2 instance, otherwise the system won't know where our EC2 instance is and which one to connect to. Copy the address that appears when you click on the instance details.

<img width="265" alt="Screenshot 2025-05-23 at 9 43 26 PM" src="https://github.com/user-attachments/assets/9e9224f1-7513-48b9-b882-edbef0c6c617" />

Now, to connect to the EC2 instance, use this command:

```bash
ssh -i [Path name to the private key] ec2-user@[Public DNS server address]
```

Type 'yes' in the terminal to connect.

We can see the change in the name of the root user in the terminal

<img width="504" alt="Screenshot 2025-05-24 at 9 30 34 PM" src="https://github.com/user-attachments/assets/a1628b4f-d9c2-4975-af9e-858c69d1bc43" />

## *Creating the Web app*

*Apache Maven* is a tool that helps developers build and organize Java software projects. It's also a package manager, which means it will automatically download any external pieces of the code that your project depends on to work.

It uses archetypes, which are like templates, that are used to lay out the foundations for different types of projects, such as web apps.

Java is required for this project because we are using Apache Maven, which relies on Java to develop and build our web application.

Now, we have to install Maven in the EC2 instance which can be done using the commands below :

```bash
wget https://archive.apache.org/dist/maven/maven-3/3.5.2/binaries/apache-maven-3.5.2-bin.tar.gz
sudo tar -xzf apache-maven-3.5.2-bin.tar.gz -C /opt
echo "export PATH=/opt/apache-maven-3.5.2/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```
Now we're going to install Java 8, or more specifically, Amazon Correto 8.
Run these commands shown below :

```bash
sudo dnf install -y java-1.8.0-amazon-corretto-devel
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64
export PATH=/usr/lib/jvm/java-1.8.0-amazon-corretto.x86_64/jre/bin/:$PATH
```

Run Maven commands in your terminal to generate a Java web app.
Use mvn to generate a Java web app:

```bash
mvn archetype:generate \
   -DgroupId=com.javaweb.app \
   -DartifactId=javaweb \
   -DarchetypeArtifactId=maven-archetype-webapp \
   -DinteractiveMode=false
```
Output:

<img width="499" alt="Screenshot 2025-05-24 at 10 41 35 PM" src="https://github.com/user-attachments/assets/2b8cbc07-043c-4ac9-b881-2115093363a3" />

Using VS Code's file explorer, I could see all the files that have been created inside the EC2 instance including those files that I have created using Apache Maven.

Two of the project folders created by Maven are src and webapp, which have different functionalities. The "src" folders has all the source codes that define the web app and the "webapp" folder is a sub folder of the src which has the web files like CSS.

<img width="152" alt="Screenshot 2025-05-24 at 10 43 09 PM" src="https://github.com/user-attachments/assets/a90b854a-378c-401c-9b9e-b998de9a6493" />

# Opening VS code inside the EC2 Instance

Opening VS code inside the EC2 Instance can be made possible by using the Remote-SSH extension in the VS code.

After installing the extension, click on the extension which you can see at the left side bottom of the Vs code. After clicking we can view this window:

<img width="593" alt="Screenshot 2025-05-27 at 7 56 59 PM" src="https://github.com/user-attachments/assets/e6acd39a-d341-47b6-aaed-ccb32f5263bc" />

Click on "Connect to Host". Then,

<img width="587" alt="Screenshot 2025-05-27 at 7 58 48 PM" src="https://github.com/user-attachments/assets/2424e68b-64f8-4026-bb28-0980c9a03ab8" />

Click on "Add New SSH host" to add the EC2 instance to the "Config file"

<img width="600" alt="Screenshot 2025-05-27 at 7 59 09 PM" src="https://github.com/user-attachments/assets/be62c369-8e22-4292-9c18-320665d049c8" />

To connect we need to enter the same command that we entered to connect the EC2 instance to our terminal.

```bash
ssh -i [Path name to the private key] ec2-user@[Public DNS server address]
```
Now, there will be a new window that will be opened inside the ec2 instance where we can see the web app and it's folders.

# Editing the web app 

This is just a template of a web app, but we can also build and deploy any functional application on this EC2 instance.

<img width="495" alt="Screenshot 2025-05-27 at 7 50 56 PM" src="https://github.com/user-attachments/assets/937d2dd6-8abe-46b4-818b-d4e3593d7f65" />

Index.jsp is similar to HTML but it can also have Java code which can be used to show dynamic web content instead of a static one. Edited the index.jsp file by updating the HTML code using the VS Code IDE on my EC2 instance.

# Using Terminal to edit the web app

Navigate to the project folder using the terminal and then type this command:

```bash
nano index.jsp
```
where we can edit the file and save it.
