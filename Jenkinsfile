pipeline {
    agent any
    tools{
        jdk  'JAVA_HOME'
        maven  'MAVEN_HOME'
        git     'git'
        
    }
    
    environment{
        SCANNER_HOME= tool 'SCANNER_HOME'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, credentialsId: '15fb69c3-3460-4d51-bd07-2b0545fa5151', poll: false, url: 'https://github.com/ashafi-02/Shopping-Crt.git'
            }
        }
        
        stage('COMPILE') {
            steps {
                sh "mvn clean compile -DskipTests=true"
            }
        }
        
                
        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('SCANNER_HOME'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Shopping-Cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        
        stage('Docker Build & Push') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '2fe19d8a-3d12-4b82-ba20-9d22e6bf1672', toolName: 'docker') {
                        
                        sh "docker build -t shopping-cart -f docker/Dockerfile ."
                        sh "docker tag  shopping-cart adijaiswal/shopping-cart:latest"
                        sh "docker push adijaiswal/shopping-cart:latest"
                    }
                }
            }
        }
        
        
    }
}
