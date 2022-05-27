# Tools needed to deploy my infrastructure
. AWS EC2 Instance  
. AWS Elastic IP address 
. Git Bash
. GitHub
. PM2
. NGINX
. Visual Studio Code
. Docker

# Steps needed to repeat deployment process
This app was deployed in AWS. This project focusses on setting up the app server on AWS EC2, then deploying and configuring the front-end and back-end pieces to work together.

 **Step 1: Lauch an Ubuntu Server 22.04 LTS in AWS** 

 . Sign into the AWS Console at https://aws.amazon.com/console/.(Create a free account if you do not have one.)
 . In the search bar, searchfor EC2 service
 . Select and click the " Launch Instances" and follow the prompts to launch your instance
 . in the AMI section, be sure to check the "Free tier only" to display the free AMIs
 . Select the Ubuntu Server 22.04 LTS image
 . Under instance type, select the "t2.micro" (Free tier eligible) instance type and click next to configure instance details( leave the default settings).
 . Click next to storage (default) and next to Tags(optional but beneficial for identification. I'd suggestion a Name and Owner tag configuration.
 . Click next to configure the Security Group. Add anew rule to allow HTTP Traffic.
 . Click Review and Launch your instance
 . Select "Create a new key pair", enter a name for the key pair (e.g. "dev-code") and click "Download Key Pair" to download the private key, you will use this to connect to the server via SSH.
 . Click "Launch Instances", then scroll to the bottom of the page and click "View Instances" to see details of the new Ubuntu EC2 instance 
 
 PS:Allocate an elastic IP to your instance. AWS Ec2 Ips change after the instance is stopped/re-started. The Elastic IP is static.
 ##To allocate an EIP(Elastic IP), click on Elastic IP and click on "Allocate Elastic IP Address" >> Scroll down to select "Allocate" >> Select the new EIP >> click on "Actions" >> select "Associate Elastic IP address" >> Associate the ip to the instance. You're set!

 **Step 2: Ensure the EC2 instance is runnig before proceeding with below steps**

 . We will connect to the EC2 instance via ssh using the keypair we downladed while launching our instance
 . Open an ssh tool. I will be using VSCode
 . Locate your private key file. If not moved, the key should be in your downloads folder.
 
 . $ cd Downloads
   $ chmod 400 "keyname"
   $ ssh -i "keyname" ubuntu@PUBLIC_IPV4_DNS
  PS: Please note that you can select your EC2 instance and click "Connect" to see the different options to connect to your EC2. We will be connecting using an SSH client 
You're now logged into your ubuntu server.

 **Step 3: Setup your environment**

 . Proceed with a system update and upgrade
   sudo apt-get update

   Run the command 
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash 
sudo apt-get install -y nodejs

#To compile and install native add-ons from npm you may also need to install build tools:

sudo apt-get install -y build-essential

#Install nodejs. Binaries and installers can be found on nodejs.org.
https://nodejs.org/en/download/

For macOS or Linux, Nodejs can usually be found in your preferred package manager.
https://nodejs.org/en/download/package-manager/

##Depending on the Linux distribution, the Node Package Manager `npm` may need to be installed separately.##

#Install NPM, NGNIX
sudo npm install node 
sudo apt-get install -y nginx 

**Configure Nginx: Use your AWS EC2 EIP address for the server_name**
```
sudo touch /etc/nginx/sites-available/devops-code-challenge
echo \
"server {
        listen 80;
        server_name YOUR EIP;
        location / {
                proxy_pass http://localhost:3000;
        }
        location /api/ {
                proxy_pass http://localhost:8080;
        }
}" | \
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/devops-code-challenge /etc/nginx/sites-enabled/
sudo systemctl restart nginx

**Step 4: Running the project**

Clone the Devops-Code-Challenge App repo from your GitHub

git clone "YOUR GIT CLONE CODE(from the forked repo)"

#Start the backend process

cd ~/devops-code-challenge/backend
npm ci
pm2 start npm --name "backend" -- start

#start the frontend process
cd ~/devops-code-challenge/frontend
npm ci
pm2 start npm --name "frontend" -- start

Go to http://[ELASTICIP ADDRESS] for frontend and http://[ELASTICIP ADDRESS]/api for backend.

# Access my deployed application using the below links
Frontend: http://34.239.82.77/


Backend: http://34.239.82.77/api

