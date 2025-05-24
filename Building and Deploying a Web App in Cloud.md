# Build and Deply a Web App in Cloud

## Launching an EC2 Instance
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

#### *3.1 Download the key as a Pem File * ####

Pem stands for Privacy Enhanced Mail that we can use 


