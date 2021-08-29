pipeline {
    agent any
	
    environment {
	docker_username_password_creid='b4d22508-ebd0-44fe-b575-98805582d7ef'
	dockerhub_url='registry.hub.docker.com'
    }
    stages {
        stage('pull code') {
            steps {
                echo 'pull code from github'
                checkout([$class: 'GitSCM', branches: [[name: '*/${branch}']], extensions: [], userRemoteConfigs: [[credentialsId: 'e3bcb365-4b0a-4d60-9b10-8a1e48117bf3', url: 'git@github.com:kexiaomeng/devops-java-sample.git']]])
            }
        }
        // 这一步可以不要
        //stage('code analysis') {
          //  steps { 
             //   script {
                    // 引入sonarqube-scanner工具
                   // scannerHome = tool 'sonar-scanner'
              //  }
                // 括号引入sonarqube服务器环境的名称
             //  withSonarQubeEnv('sonarqube') {
                   // bat "${scannerHome}/bin/sonar-scanner"
                //}
           // }
            
       // }
        // 编译打包项目
        stage('build project') {
            steps { 
                echo 'build project'
                bat 'mvn -Dmaven.test.skip=true clean package'
            }
            
        }
	// 编译镜像
	stage('build images and push'){
			steps { 
				echo 'build docker images'
				// 制作镜像
				bat 'docker build -f DockerFile -t kexiaomeng/devops-java-sample:v1.0 .'
				// 将镜像打标签
				bat 'docker tag kexiaomeng/devops-java-sample:v1.0 kexiaomeng824/devops-java-sample:v1.0'
				// 推送镜像
				withCredentials([usernamePassword(credentialsId: "${docker_username_password_creid}", passwordVariable: 'password', usernameVariable: 'username')]) {
					// some block
					// 登录到dockerhub
					bat "docker login -u ${username} -p ${password} ${dockerhub_url}"
					bat "docker push kexiaomeng824/devops-java-sample:v1.0"
				}
				echo 'push success'
			}
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
