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

