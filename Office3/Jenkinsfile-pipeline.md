# Some Pipeline Scripts

*** Basic Scripted Pipeline***
```java
  node {
    stage('Build') {
      echo "Build"
    }
    stage('Test') {
      echo "Test"
    }
    stage('Integration Test'){
      echo "Integration Test"
    }
  }

```
In the Scripted Pipeline Approach these stages are not mandatory. So we will remove the **stages** and also check
```java

node {
      echo "Build"
      echo "Test"
      echo "Integration Test"
  }
```
*** Basic Declarative Pipeline Running on any stage***
```java

pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                echo "Build"
            }
        }
        stage('Test'){
            steps {
                echo "Test"
            }
        }
        stage('Integration Test'){
            steps{
                echo "Integration Test"
            }
        }
    } 

post {
        always {
            echo "I am awesome. I run always"
        }
        success {
            echo " I run when you are successful"
        }
        failure {
            echo " I run when you fail"
        }
    }

}
```
We can also define specific *** agent*** where the build would run
```java
  agent {
          docker { image "maven:3.6.3"}
      }
```
After Specifying Agent
```java
pipeline {
    agent {
        docker { image "maven:3.6.3"}
    }
    stages{
        stage('Build'){
            steps {
                sh 'mvn --version'
                echo "Build"
            }
        }
        stage('Test'){
            steps {
                echo "Test"
            }
        }
        stage('Integration Test'){
            steps{
                echo "Integration Test"
            }
        }
    } 
    
    post {
        always {
            echo "I am awsome. I run always"
        }
        success {
            echo " I run when you are successful"
        }
        failure {
            echo " I run when you fail"
        }
    }
}
```
Now we want to use some environment variables.
We will find those in **pipeline syntax** , in **global variable reference**
```java
pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                echo "Build"
                echo "PATH=$PATH"
                echo "BUILD_NUMBER=$env.BUILD_NUMBER"
                echo "JOB_NAME =$env.JOB_NAME"
                echo "BUILD_TAG =$env.BUILD_TAG"
            }
        }
    }
}    
```

And the output:
```java
PATH=/usr/local/openjdk-8/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
[Pipeline] echo
BUILD_NUMBER=3
[Pipeline] echo
JOB_NAME =jenkins-devops-microservice-pipelines
[Pipeline] echo (hide)
BUILD_TAG =jenkins-jenkins-devops-microservice-pipelines-3
```

Now by using **Maven** and **Docker** available  on the build Machine
```java
pipeline {
    agent any
    environment {
        dockerHome = tool "myDocker"
        mavenHome = tool "myMaven"
        PATH ="$dockerHome/bin:$mavenHome/bin:$PATH"
    }
    stages{
        stage('Build'){
            steps {
                sh "mvn --version"
                sh "docker --version"
            }
        }
    }
}    
```

## The full Pipeline Script

Repo it Refers: https://github.com/kingshuknandy2016/Jenkins-devops-microservice-complete

```java
pipeline {
    agent any
    environment {
        dockerHome = tool "myDocker"
        mavenHome = tool "myMaven"
        PATH ="$dockerHome/bin:$mavenHome/bin:$PATH"
    }
    stages{
        stage('Checkout'){
            steps {
                sh "mvn --version"
                sh "docker version"
                echo "BUILD"
                echo "PATH = $PATH"
                echo "BUILD_NUMBER = $env.BUILD_NUMBER"
                echo "BUILD_ID = $env.BUILD_ID"
                echo "JOB_NAME =$env.JOB_NAME"
                echo "BUILD_TAG =$env.BUILD_TAG"
                echo "BUILD_URL = $env.BUILD_URL"
            }
        }
        stage('Compile'){
            steps{
                sh """cd currency-exchange-microservice
                    mvn clean compile"""
            }
        }
        stage('Test'){
            steps{
                sh """cd currency-exchange-microservice
                mvn test"""
            }
        }
        stage('Integration Test'){
            steps{
                sh """cd currency-exchange-microservice
                mvn failsafe:integration-test failsafe:verify"""
            }
        }
        stage('Package'){
            steps{
                sh """cd currency-exchange-microservice
                mvn package -DskipTests"""
            }
        }
        stage('Build Docker Image'){
            steps{
                //docker build -t kingshuknandy/currency-exchange:0.0.1.RELEASE .
                script{
                    dockerImage=docker.build("kingshuknandy/currency-exchange:${env.BUILD_TAG}")
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    docker.withRegistry('','dockerhub'){
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}    
```
