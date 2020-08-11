## Hello Springboot deployment using CICD Pipeline with helm

## Pre-requisites:
    - Install Java
    - Install Git
    - Install Maven
    - Install Docker
    - Install Jenkins
    - Install Helm
    - EKS Cluster
## Install Java:
    yum install java-1.8.0-openjdk-devel -y
## Install Git:
    yum install git -y
## Install Apache-Maven:
    wget https://mirrors.estointernet.in/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
    tar xvzf apache-maven-3.6.3-bin.tar.gz
    
    vi /etc/profile.d/maven.sh
    --------------------------------------------
    export MAVEN_HOME=/opt/apache-maven-3.6.3
    export PATH=$PATH:$MAVEN_HOME/bin
    --------------------------------------------
    
    source /etc/profile.d/maven.sh
    mvn -version
## Install Jenkins
    sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import http://pkg.jenkins.io/redhat-stable/jenkins.io.key
    sudo yum install jenkins
    service jenkins start
## Install Docker:
    yum install docker -y
    service docker start
    usermod -aG docker jenkins
## Jnekins Restart
    service jenkins restart
## Install helm
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh
## Goto Jenkins and add plugins
    jenkins --> Manage Jenkins --> Available
    - CloudBees AWS Credentials
    - Kubernetes Credentials
    - Kubernetes Continuous Deploy
## Create Credentials
    - docker credentials
             (credentialsId: 'docker_credentials',  username: 'username', password: 'password')
    - kubernetes config credentials
             (credentialsId: 'kube_config', variable: 'KUBECONFIG')
    - aws configure credentials
             (accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws_configure', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')
## Create Github webhook
![image](https://user-images.githubusercontent.com/68885738/89870155-0ebbea00-dbd3-11ea-837a-12c02c2c6037.png)
![image](https://user-images.githubusercontent.com/68885738/89870125-0368be80-dbd3-11ea-9439-7f7e83b7c2fc.png)

And one more step we need to do in Github

![image](https://user-images.githubusercontent.com/68885738/89870357-5773a300-dbd3-11ea-9ae2-ea830f415967.png)

## Add JenkinsFile Content inside pipeline section
![image](https://user-images.githubusercontent.com/68885738/89870517-94d83080-dbd3-11ea-8643-ab84074737f8.png)

Now we can try by change anything inside Github

## Check output of application --> <loadbalancer>:8080/hello
    
        http://a66329da6d1804b61bdf5318a40ce772-1936783521.us-east-1.elb.amazonaws.com:8080/hello
