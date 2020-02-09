1. Create roles
Create roll with fullAdmin Acces on ECR with fill access on container and container registery

2. Create EC2
2. Update the installed packages and package cache on your instance
   sudo yum update -y
4. Install Docker 
   sudo amazon-linux-extras install docker
   or
   sudo yum install docker -y
5. start docker service
   sudo service docker start
6. verify if ec2-user run docker command without sudo
   docker info

#--------------- Optional because this we are creating through pipeline
Create Docker Image
1. Create dockerfile
   touch dockerfile
2. Edit docker file
   
    FROM ubuntu:18.04

    # Install dependencies
    RUN apt-get update && \
    apt-get -y install apache2

    # Install apache and write hello world message
    RUN echo 'Hello World!' > /var/www/html/index.html

    # Configure apache
    RUN echo '. /etc/apache2/envvars' > /root/run_apache.sh && \
    echo 'mkdir -p /var/run/apache2' >> /root/run_apache.sh && \
    echo 'mkdir -p /var/lock/apache2' >> /root/run_apache.sh && \ 
    echo '/usr/sbin/apache2 -D FOREGROUND' >> /root/run_apache.sh && \ 
    chmod 755 /root/run_apache.sh

    EXPOSE 80

    CMD /root/run_apache.sh

3. Build Docker Image
   docker build -t hello-world .

4. Run docker image to verify that the image was created correctly
   docker images --filter reference=hello-world

5. Run newly build image
   docker run -t -i -p 80:80 hello-world


#--------------------------------------------------------
Create Repository
1. using AWS CLI or by command
   aws ecr create-repository \
    --repository-name sample-repo \
    --image-scanning-configuration scanOnPush=true

Push images
1. list image
   docker images

2. Tag the image to push to your repository.
   docker tag ubuntu:trusty aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
   eg: docker tag hello-world 440535814002.dkr.ecr.us-east-1.amazonaws.com/fargate  

3. login
   aws ecr get-login --no-include-email --region us-east-1  
   copy paste above command output for successfull login

4. Push image  $ docker push aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
   $ docker push aws_account_id.dkr.ecr.us-east-1.amazonaws.com/ubuntu:trusty
   eg: docker push 440535814002.dkr.ecr.us-east-1.amazonaws.com/fargate

Create Cluster
1. Create cluster powered by Fargate

2. define a task definition

3. add container

reference link
https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html

#-------------------------------------------------------------------------------
ECR configuration

1. Create ECR repository

#-------------------------------------------------------------------------------

Jenkins Server

1. install JAVA

2. set environment path $JAVA_HOME

3. Get the latest version of jenkins
    yum -y install wget
    sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
    yum -y install jenkins

4. Start Jenkins
    service jenkins start

5. Configure Jenkins
    The default Username is admin
    Grab the default password
    Password Location:/var/lib/jenkins/secrets/initialAdminPassword
    Skip Plugin Installation; We can do it later
    Change admin password
    Admin > Configure > Password
    Configure java path
    Manage Jenkins > Global Tool Configuration > JDK
    Create another admin user id

6. Install git on Jenkin Server
   yum install git

7. Add docker and github Plugin in jenkins using manage Plugin

8. EC2 create docker user for Jenkins
    usermod -aG docker jenkins
    usermod -aG root jenkins
    chmod 664 /var/run/docker.sock

    chmod 777 /var/run/docker.sock

9. AWS credential configuration
   add secret key and access id that will be used when we push image in ECR
   
NOTE: we can create parameter also to pass value in Jenkins dynamically 