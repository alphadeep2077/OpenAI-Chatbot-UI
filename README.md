# OpenAI-Chatbot-UI

# Introduction :

In this project, we aim to implement  deploying an OpenAI Chatbot UI. We will use Kubernetes (EKS) for container orchestration, Jenkins for Continuous Integration/Continuous Deployment (CI/CD), and Docker for containerization.

## What is ChatBOT? 

ChatBOT is an AI-powered conversational agent trained on extensive human conversation data. It utilizes natural language processing techniques to understand user queries and provide human-like responses. By simulating natural language interactions, ChatBOT enhances user engagement and provides personalized assistance to users.

## Why ChatBOT?

1.   Personalized Interactions: ChatBOT enables personalized interactions by understanding user queries and responding in a conversational manner, fostering engagement and satisfaction.

2.  24/7 Availability: Unlike human agents, ChatBOT is available 24/7, ensuring instant responses to user queries and delivering a seamless user experience round the clock.

3.  Scalability : With ChatBOT deployed in our application, we can efficiently handle a large volume of user interactions, ensuring scalability as our user base expands.

## How We’re Deploying ChatBOT?

1. Containerization with Docker: We’re containerizing the ChatBOT application using Docker, which provides lightweight, portable, and isolated environments for running applications. Docker enables consistent deployment across different environments, simplifying the deployment process and ensuring consistency.

2. Orchestration with Kubernetes (EKS): Kubernetes provides powerful orchestration capabilities for managing containerized applications at scale. We’re leveraging Amazon Elastic Kubernetes Service (EKS) to deploy and manage our Docker containers efficiently. EKS automates container deployment, scaling, and management, ensuring high availability and resilience.

3. CI/CD with Jenkins: Jenkins serves as our CI/CD tool for automating the deployment pipeline. We’ve configured Jenkins to continuously integrate code changes, run automated tests, and deploy the ChatBOT application to EKS. By automating the deployment process, Jenkins accelerates the delivery of updates and enhancements, improving efficiency and reliability.

4. DevSecOps Practices: Throughout the deployment pipeline, we’re integrating security practices into every stage to ensure the security of our ChatBOT application. This includes vulnerability scanning, code analysis, and security testing to identify and mitigate potential security threats early in the development lifecycle.

By implementing DevSecOps practices and leveraging modern technologies like Kubernetes, Docker, and Jenkins, we’re ensuring the secure, scalable, and efficient deployment of ChatBOT, enhancing user engagement and satisfaction.

# STEPS:

Step:1 :- Create Jenkins Server.

Clone the GitHub repository.

```bash
git clone https://github.com/NotHarshhaa/DevOps-Projects/DevOps-Project-28/Chatbot-UI
cd Jenkins-Server-TF
```

2. Before proceeding to next steps. Do the following things.

i. Create a DynamoDB table named “Lock-Files”.
ii. Create a Key-Pair and download the PEM file.
iii. Create a user and save the access keys.
iv. Create an S3 bucket.
v. Download Terraform and AWS CLI.


```bash
#Terraform Installation Script
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg - dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y

#AWSCLI Installation Script
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

3. Do some modifications to the backend.tf file such as changing the bucket name and DynamoDB table.


![image](https://github.com/user-attachments/assets/d5b7f628-49d0-499b-9b1e-0ac701922c9c)

4. Configure AWS CLI: Run the below command, and add your access keys.

```bash
aws configure
```

5. You have to replace the Pem File name with one that is already created on AWS.

6. Initialize the backend by running the below command.

```bash
terraform init
```


![image](https://github.com/user-attachments/assets/1e2b49ac-a3cd-40c1-ab7c-e941ca631e9e)

7. Run the below command to get the blueprint of what kind of AWS services will be created.

```bash
terraform plan -var-file=variables.tfvars
```


![image](https://github.com/user-attachments/assets/5065dc49-71dd-4a23-8763-b2f12e704fdc)

8. Now, run the below command to create the infrastructure on AWS Cloud which will take 3 to 4 minutes maximum.

```bash
terraform apply -var-file=variables.tfvars --auto-approve
```


![image](https://github.com/user-attachments/assets/35c10dac-4f2c-4aea-b248-1705b33c9ca9)

9. Upon success,this will create an ec2 server with name “Jenkins-server”.


![image](https://github.com/user-attachments/assets/a5e8ada8-b8be-43e8-950d-891d1064d3ee)

10. Connect to it with SSH.


![image](https://github.com/user-attachments/assets/81e26177-088d-41a8-90b8-be831e70380c)

Step:2 :- Configure Jenkins server.

Access jenkins on port 8080 of ec2 public ip.


![image](https://github.com/user-attachments/assets/8d813c44-fa2d-43ff-915b-e948e742dd3c)

2. Now, run the below command to get the administrator password and paste it on your Jenkins.

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```



![image](https://github.com/user-attachments/assets/58140d3d-0c11-4c2c-ad6d-1515126365d7)

3. Install suggested plugins.


![image](https://github.com/user-attachments/assets/01eeed94-5313-4062-9a08-8c203cb50fda)

4. Create user help us customize username and password of Jenkins.


![image](https://github.com/user-attachments/assets/849db04d-3757-41bf-9dd9-e6d440349f44)

5. Save and Continue all the rest.


![image](https://github.com/user-attachments/assets/90606a0b-e417-4fa7-928a-c45269745b95)

6. Access SonarQube on port 9000 of Same Jenkins server.

```bash
Username: admin
Password: admin
```


![image](https://github.com/user-attachments/assets/30e78c5e-97ae-4c74-80ad-09a3f4de9ff5)


7. Customize the password.


![image](https://github.com/user-attachments/assets/1046217c-d4a0-4f2f-99ea-9ed688445d7f)

8. Navigate to Security → Users →Administrator.



![image](https://github.com/user-attachments/assets/90a43c6f-2154-4dab-88c0-e7a82759af77)

9. Click on Tokens.


![image](https://github.com/user-attachments/assets/27f4d19a-b8a2-49e4-b372-318ca7246bdf)

10. Create one and save it.


![image](https://github.com/user-attachments/assets/ea801b79-b852-42e9-b4b9-d5310fa9dc16)

11. Now go to Configuration → Webhooks →Create


![image](https://github.com/user-attachments/assets/52c9fd92-4878-4252-8d10-52c01bdf55b2)

12. Then configure webhook and create.

```bash
Name: Jenkins
URL : http://<public_ip>:8080/sonarqube-webhook/
```


![image](https://github.com/user-attachments/assets/fe4b0441-5e4f-4151-8b7b-08147df2eccf)

13. Now in Jenkins Navigate to Manage Jenkins → Plugins →Available Plugins and install the following.

```bash
1 → Eclipse Temurin Installer

2 → SonarQube Scanner

3 → NodeJs Plugin

4 → Docker

5 → Docker commons

6 → Docker pipeline

7 → Docker API

8 → Docker Build step

9 → Owasp Dependency Check

10 → Kubernetes

11 → Kubernetes CLI

12 → Kubernetes Client API

13 → Kubernetes Pipeline DevOps steps

10 → AWS Credentials

11 → Pipeline: AWS Steps
```

![image](https://github.com/user-attachments/assets/5cb7e3c1-719d-4a99-bd08-71ad8e817206)

14. Restart Jenkins after they got installed.


![image](https://github.com/user-attachments/assets/6ea79fbb-3170-4fac-90e2-25e8f38f70da)


15. Go to Manage Jenkins → Tools → Install JDK(17) and NodeJs(19)→ Click on Apply and Save.


![image](https://github.com/user-attachments/assets/1115d348-5aac-4066-a4fd-118fe5894870)


![image](https://github.com/user-attachments/assets/66e43be3-858b-41af-841e-158477766808)

16. Similarly install DP-check, Sonar-Scanner and Docker.


![image](https://github.com/user-attachments/assets/98b4677b-1d9d-406f-b896-cbba3972fb13)


17. ![image](https://github.com/user-attachments/assets/14c6c72f-6797-4fb5-bd93-5445c0f9b32c)

![image](https://github.com/user-attachments/assets/6879c59b-d453-4292-ac7d-918607e434fb)

![image](https://github.com/user-attachments/assets/d2b114b7-2033-41c1-a8e0-66d2bbf7c381)

18. Go to Jenkins Dashboard → Manage Jenkins → Credentials. Add
Sonar-token as secret text.


![image](https://github.com/user-attachments/assets/8bf399f3-13e0-4776-bf4a-0f529959eba2)

Docker credentials.


![image](https://github.com/user-attachments/assets/36c9b784-d68f-427d-8083-b827a387e72f)

GitHub Credentials.


![image](https://github.com/user-attachments/assets/d05b60cd-f53c-4c9f-a7ed-29b91792e954)

AWS access keys as AWS Credentials.


![image](https://github.com/user-attachments/assets/b55ed13d-4073-403d-8c64-42d1e7bf2b09)

19. Manage Jenkins → Tools → SonarQube Scanner. Then add sonar-server and created sonar-token.


![image](https://github.com/user-attachments/assets/931e800d-5f18-4f98-944e-4a8e65e9ddae)

Step :3 :- Create Jenkins Pipeline

Up to this Let’s create a pipeline and see if anything gone wrong.
Click on “New Item” and give it a name selecting pipeline and then ok.


![image](https://github.com/user-attachments/assets/6bdcdd82-fae2-44fe-aa0a-ba6eed7636de)

2. Under Pipeline section Provide

```bash
Definition: Pipeline script from SCM
SCM : Git
Repo URL : Your Github Repo 
Credentials: Created GitHub Credentials
Branch: Main
Path: Your Jenkinsfile path in GitHub repo.
```


![image](https://github.com/user-attachments/assets/12a98a61-15c9-4347-b8a2-edeba6ab2772)


![image](https://github.com/user-attachments/assets/e15007ba-5741-47ec-97b0-57b2b5260fa6)

3. Click on “Build”.

Upon successful execution you can see all stages as green.


![image](https://github.com/user-attachments/assets/0a511b9c-a1ea-4497-a8e1-2b587bb0ef65)
Sonar- Console.


![image](https://github.com/user-attachments/assets/d2115233-b30d-4f6b-953b-582cee89f937)

Dependency Check:


![image](https://github.com/user-attachments/assets/fc0e3a5a-ea4e-47e8-bbfb-5e36d2daf487)

Docker Hub:


![image](https://github.com/user-attachments/assets/df4a419a-8554-4552-9e4e-8422559cdaa5)

4. Now access the application on port 3000 of Jenkins public ip.
Note: Make sure you allowed port 3000 in Security Group of Jenkins Server.


![image](https://github.com/user-attachments/assets/5c7a9156-6821-454c-b8b8-9edc7e4f1ddc)

5. Click on openai.com(Blue in color)
This will take you ChatGPT login page enter email and password.
In API Keys Create Click on new secret key.



![image](https://github.com/user-attachments/assets/f83dd875-e6fd-4dde-a756-55d43cae1b43)

Give a name and copy it.


![image](https://github.com/user-attachments/assets/3e5fd0dd-8191-45e5-81c8-59ef3854f202)

6. Come back to chatbot UI that we deployed and bottom of the page you will see OpenAI API key and give the Generated key and click on save (RIGHT MARK).


![image](https://github.com/user-attachments/assets/231fc6b0-6198-411d-9e55-18af32798dec)

UI look like:


![image](https://github.com/user-attachments/assets/44d598cb-e62f-4110-a4cc-75cd16cb0430)

7. Now, You can ask questions and test it.


![image](https://github.com/user-attachments/assets/fcc8ca7c-427e-4f4f-acf3-734e01d2f9d7)

Step:4 :- Create EKS Cluster withy Jenkins

Click on New item give it a name by choosing pipeline and then click “OK”.


![image](https://github.com/user-attachments/assets/142d2655-3c25-4489-930b-d3a673fda115)

2. Under Pipeline section provide:

```bash
Definition: Pipeline script from SCM
SCM : Git
Repo URL : Your Github Repo 
Credentials: Created GitHub Credentials
Branch: Main
Path: Your EKS Cluster Jenkinsfile path in GitHub repo.
```


![image](https://github.com/user-attachments/assets/94cdcc1d-3b40-49c9-b2a6-8d9cb55d7258)


![image](https://github.com/user-attachments/assets/31a879b0-337f-4bfc-9453-854039d5b77a)

3. Click on Build and on successful execution Your Jenkins UI resembles:


![image](https://github.com/user-attachments/assets/3a9ae7b6-3926-47de-bc49-d05ccf981a10)

This will create a cluster in AWS:


![image](https://github.com/user-attachments/assets/a4c920f2-6dfc-4589-8277-dc8fe4cc1272)

4. Now In the Jenkins server. Give this command to add context.

```bash
aws eks update-kubeconfig --name <clustername> --region <region>
```

5. It will Generate an Kubernetes configuration file.
Navigate to the path of config file and copy it.'

```bash
cd .kube
cat config
```

7. Now in Jenkins Console add this file in Credentials section with id k8s as secret file.


![image](https://github.com/user-attachments/assets/ced7589c-07bc-4baf-9bfb-0e4a179962f0)

Step:5 :- Deployment on EKS

1. Now add this deployment stage in Jenkins file.
```bash
stage('Deploy to kubernetes'){
            steps{
                withAWS(credentials: 'aws-key', region: 'us-east-1'){
                script{
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                       sh 'kubectl apply -f k8s/chatbot-ui.yaml'
                  }
                }
            }
        }
     }
```

2. Now rerun the Jenkins Pipeline again.

Upon Success:


![image](https://github.com/user-attachments/assets/0ec07016-7b9a-400d-a2f2-4df4712b416e)

This create all the resources in the Cluster:

```bash
kubectl get all
```
This will create an Classic Load Balancer on AWS Console:

![image](https://github.com/user-attachments/assets/2e42510d-c76c-445f-8055-3017d5d6b622)

Copy the DNS Name and Paste it on your browser and use it:

Note: Do the same process and add key to get output.


![image](https://github.com/user-attachments/assets/7abbed8c-6c73-4ba7-a814-8e259e951bc2)


![image](https://github.com/user-attachments/assets/f9658512-2f01-44ca-8754-17a282287659)

The Complete Jenkins file:

```bash
pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node19'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'master', url: 'https://github.com/NotHarshhaa/DevOps-Project-28/Chatbot-UI.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chatbot \
                    -Dsonar.projectKey=Chatbot '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
                }
            } 
        }
        }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t chatbot ."
                       sh "docker tag chatbot ProDevOpsGuyTech/chatbot:latest "
                       sh "docker push ProDevOpsGuyTech/chatbot:latest "
                    }
                }
            }
        }
        }
        stage ("Remove container") {
            steps{
                sh "docker stop chatbot | true"
                sh "docker rm chatbot | true"
             }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name chatbot -p 3000:3000 sreedhar8897/chatbot:latest'
            }
        }
        stage('Deploy to kubernetes'){
            steps{
                withAWS(credentials: 'aws-key', region: 'us-east-1'){
                script{
                    withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                       sh 'kubectl apply -f k8s/chatbot-ui.yaml'
                  }
                }
            }
        }
      }
   }
}
```

Step: 6 :- Clean Up
This is so simple Firstly Delete the EKS Cluster By selecting destroy as the build option.

![image](https://github.com/user-attachments/assets/1fbdfe66-cdd7-4a3d-bff0-7a9f9c246de3)

2. Then destroy Jenkins server by running this on Local:

```bash
terraform destroy -auto-approve -var-file=variables.tfvars
```

