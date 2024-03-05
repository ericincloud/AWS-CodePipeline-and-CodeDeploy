# AWS-CodePipeline-and-CodeDeploy
#### 

## Architectural Diagram

## Step 1: Create 2 IAM roles: CodeDeploy EC2 Role AND CodeDeploy Role
#### 

## Step 2: Setup EC2 instance
#### Create an EC2 instance in a public subnet. Configure the instance with the EC2 CodeDeploy Role instance profile. 

#### Install the NGINX webserver. Use commands: 
```
sudo yum update
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl enable nginx

# Check status:
sudo systemctl status nginx
```

#### Edit the EC2's security group to allow Port 80 from anywhere. 

#### Install the CodeDeploy Agent if needed using commands: 
```
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
chmod +x install
sudo ./install auto
sudo systemctl start codedeploy-agent
sudo systemctl enable codedeploy-agent

# Check status:
sudo systemctl status codedeploy-agent

```

#### Verify functionality by accessing the instance IP. You should see a default NGINX webpage.

## Step 3: Create CodeDeploy Application
#### Head to CodeDeploy > Applications > Create Application

## Step 4: Create Pipeline
#### Connect to Github repository. Paste in your repo name if it does not pop up on the selection menu. > No filter> > Skip build stage > Select CodeDeploy for as the deploy provider and select the CodeDeploy Application created from Step 3. > Create pipeline. 

## Step 5: Create appspec.yml file
#### To change the contents of the webpage and include necessary resources, create `appsepc.yml` file in the GitHub respository with the following configuration: 

 ```
version: 0.0
os: linux
files:
  - source: /index.html
    destination: /usr/share/nginx/html
  - source: /style.css
    destination: /usr/share/nginx/html
  - source: /avatarmaker.png
    destination: /usr/share/nginx/html
hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300 
      runas: root
 ```

#### Create a `Scripts` folder with files `after_install.sh` and `before_install.sh`. 

#### Within the `after_install.sh` file include:

```
#!/bin/bash

# Copy updated webpage files to Nginx document root
cp -r /path/to/updated/html/files/* /usr/share/nginx/html/

# Restart Nginx service
service nginx start
```

#### Within the `before_install.sh` include:

```
#!/bin/bash

# Stop Nginx service
service nginx stop

# Remove existing webpage files
rm -rf /usr/share/nginx/html/*
```
## Step 6: Verify Functionality
#### Refresh the webpage. If everything is properly configured, you will be able to see the new webpage you've added! 

### Finish! Congratulations you've setup and configured CodeDeploy and CodePipeline!

## Step 6: 
#### 

## Step 7: 
#### 

## Notes
*

## Reference
* 
  

