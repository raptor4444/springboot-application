pipeline {
    agent any
    
    tools {
        jdk 'jdk11'
        maven 'maven3'
    }
    
    environment{
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/raptor4444/springboot-application.git'
            }
        }
        
        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        
        stage('Run Test Cases') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '935aac27-f01a-447e-a720-65b8ecbd95c5', toolName: 'Docker-Credentials') {
                        sh "docker build -t webapp ."
                        sh "docker tag webapp raptor4444/springboot-webapp:latest"
                        sh "docker push raptor4444/springboot-webapp:latest"
                    }
                }
            }
        }
        
        //below command should use seperate container, as jenkins is already running on port 8080 (Tomcat also use 8080 port by default)
        stage('Docker Container') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '935aac27-f01a-447e-a720-65b8ecbd95c5', toolName: 'docker') {
                        sh "docker run --name webapp -p 8080:8080 raptor4444/springboot-webapp" 
                    }
                }
            }
        }
        
    }
}
