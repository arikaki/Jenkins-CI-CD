# JENKINS-CI-CD

## AWS EC2 Instance.
### Steps from here to Docker config has to be done in EC2 instance terminal/shell
- Go to AWS Console
- Instances(running)
- Launch instances

## Install Jenkins.
Pre-Requisites:
 - Java17 (JDK)
### Run the below commands to install Java and Jenkins

Install Java
```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed
```
java -version
```

Proceeding with installing Jenkins
```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>.
- In the bottom tabs -> Click on Security.
- Security groups.
- Add inbound traffic rules. [add all traffic for the project]

### Login to Jenkins using the below URL:
http://<ec2-instance-public-ip-address>:8080    [ec2-instance-public-ip-address from AWS EC2 console page]

Note: If not interested in allowing `All Traffic` to the EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password

### Click on Install suggested plugins.
Wait for the Jenkins to Install suggested plugins.
Create First Admin User or Skip the step.
After Jenkins is installed successfully we can now start using the Jenkins

## Install the Docker Pipeline plugin in Jenkins:
   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.

Wait for the Jenkins to be restarted.

## Docker Slave Configuration
Run the below command to Install Docker
```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.
```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Restart Jenkins.
```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.

-------------------------------------------------------------------------------------------------------------------------




