pipeline {
    agent any

    stages {
        stage('pull code') {
            steps {
                echo 'pull code from github'
                checkout([$class: 'GitSCM', branches: [[name: '*/${branch}']], extensions: [], userRemoteConfigs: [[credentialsId: 'e3bcb365-4b0a-4d60-9b10-8a1e48117bf3', url: 'git@github.com:kexiaomeng/devops-java-sample.git']]])
            }
        }
        stage('build project') {
            steps { 
                echo 'build project'
                bat 'mvn -Dmaven.test.skip=true clean package'
            }
            
        }
        
        stage('deploy') {
            steps { 
                echo 'deploy project'
                bat 'java -jar ./target/devops-sample-0.0.1-SNAPSHOT.jar'
            }
            
        }
    }
}
