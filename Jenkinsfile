pipeline {
    agent any

    stages {
        stage('pull code') {
            steps {
                echo 'pull code from github'
                checkout([$class: 'GitSCM', branches: [[name: '*/${branch}']], extensions: [], userRemoteConfigs: [[credentialsId: 'e3bcb365-4b0a-4d60-9b10-8a1e48117bf3', url: 'git@github.com:kexiaomeng/devops-java-sample.git']]])
            }
        }
        // 这一步可以不要
        stage('code analysis') {
            steps { 
                script {
                    // 引入sonarqube-scanner工具
                    scannerHome = tool 'sonar-scanner'
                }
                // 括号引入sonarqube服务器环境的名称
                withSonarQubeEnv('sonarqube') {
                    bat "${scannerHome}/bin/sonar-scanner"
                }
            }
            
        }
        // 编译打包项目
        stage('build project') {
            steps { 
                echo 'build project'
                bat 'mvn -Dmaven.test.skip=true clean package'
            }
            
        }
		// 编译镜像
		stage('打包镜像'){
			echo 'build docker images'
			bat 'docker build -f DockerFile -t kexiaomeng/devops-java-sample:v1.0 .'
			// docker build
		}
        // 发部项目
        stage('deploy') {
            steps { 
                echo 'deploy project'
                bat 'java -jar ./target/devops-sample-0.0.1-SNAPSHOT.jar'
            }
            
        }
    }
}
