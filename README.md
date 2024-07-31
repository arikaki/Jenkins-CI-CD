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

# Jenkins Pipeline for Java based application using Maven, SonarQube, Argo CD, Helm and Kubernetes

Now to set up an end-to-end Jenkins pipeline for a Java application using SonarQube, Argo CD, Helm, and Kubernetes:

Prerequisites:

   -  Java application code hosted on a Git repository
   -  Jenkins server
   -  Kubernetes cluster
   -  Helm package manager
   -  Argo CD

Steps:

    1. Install the necessary Jenkins plugins:
       1.1 Git plugin
       1.2 Maven Integration plugin
       1.3 Pipeline plugin
       1.4 Kubernetes Continuous Deploy plugin

    2. Create a new Jenkins pipeline:
       2.1 In Jenkins, create a new pipeline job and configure it with the Git repository URL for the Java application.
       2.2 Add a Jenkinsfile to the Git repository to define the pipeline stages.

    3. Define the pipeline stages:
        Stage 1: Checkout the source code from Git.
        Stage 2: Build the Java application using Maven.
        Stage 3: Run unit tests using JUnit and Mockito.
        Stage 4: Run SonarQube analysis to check the code quality.
        Stage 5: Package the application into a JAR file.
        Stage 6: Deploy the application to a test environment using Helm.
        Stage 7: Run user acceptance tests on the deployed application.
        Stage 8: Promote the application to a production environment using Argo CD.

    4. Configure Jenkins pipeline stages:
        Stage 1: Use the Git plugin to check out the source code from the Git repository.
        Stage 2: Use the Maven Integration plugin to build the Java application.
        Stage 3: Use the JUnit and Mockito plugins to run unit tests.
        Stage 4: Use the SonarQube plugin to analyze the code quality of the Java application.
        Stage 5: Use the Maven Integration plugin to package the application into a JAR file.
        Stage 6: Use the Kubernetes Continuous Deploy plugin to deploy the application to a test environment using Helm.
        Stage 7: Use a testing framework like Selenium to run user acceptance tests on the deployed application.
        Stage 8: Use Argo CD to promote the application to a production environment.

    5. Set up Argo CD:
        Install Argo CD on the Kubernetes cluster.
        Set up a Git repository for Argo CD to track the changes in the Helm charts and Kubernetes manifests.
        Create a Helm chart for the Java application that includes the Kubernetes manifests and Helm values.
        Add the Helm chart to the Git repository that Argo CD is tracking.

    6. Configure a Sonar Server locally [EC2 instance]
        apt install unzip
        adduser sonarqube
        wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip
        unzip *
        chmod -R 755 /home/sonarqube/sonarqube-9.4.0.54424
        chown -R sonarqube:sonarqube /home/sonarqube/sonarqube-9.4.0.54424
        cd sonarqube-9.4.0.54424/bin/linux-x86-64/
        ./sonar.sh start        

    7. Configure Jenkins pipeline to integrate with Argo CD:
       7.1 Add the Argo CD API token to Jenkins credentials.
       7.2 Update the Jenkins pipeline to include the Argo CD deployment stage.

    8. Run the Jenkins pipeline:
       8.1 Trigger the Jenkins pipeline to start the CI/CD process for the Java application.
       8.2 Monitor the pipeline stages and fix any issues that arise.

This end-to-end Jenkins pipeline will automate the entire CI/CD process for a Java application, from code checkout to production deployment, using popular tools like SonarQube, Argo CD, Helm, and Kubernetes.




