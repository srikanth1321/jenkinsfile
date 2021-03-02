pipeline {

    agent {
        
        label "master"
    }
    tools{
        maven "Maven"
    }
    stages {

        stage("Git Clone") {

            steps {

                script {

                    git 'https://github.com/dstar55/docker-hello-world-spring-boot.git';

                }

            }

        }

        stage("Maven Build") {
            
            steps {

                script {
                withMaven(jdk: 'JAVA_HOME', maven: 'Maven',mavenSettingsFilePath: '/home/srikanth/.m2/settings.xml') {
                sh "mvn package -DskipTests=true"
                }
                }

            }

        }
        stage("Sonar-scan") {
            steps {
                script {
                    
                    sh "mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=Rajini@123"
                }

                }
            }
        
        stage('Build Docker Image') {
            steps {
                
                script {
      sh "docker build -t hello-world-java:${BUILD_NUMBER} ."
    
            }
        }
        }
        
        
    stage('Deploy Docker Image'){
        steps{
            
            script{
                

      sh "docker login -u admin -p Rajini@123 localhost:11004"
      sh "docker tag hello-world-java:${BUILD_NUMBER} localhost:11004/hello-world-java:${BUILD_NUMBER}"
      sh "docker push localhost:11004/hello-world-java:${BUILD_NUMBER}"
            }
        }
    }
    stage("Docker Run") {
        steps {
            
        script {
            
            sh "docker pull localhost:11004/hello-world-java:${BUILD_NUMBER}"
            sh "docker stop hello"
            sh "docker rm hello"
            sh "docker run -d -p 1178:8080 --name=hello localhost:11004/hello-world-java:${BUILD_NUMBER}"
            sh "sleep 10s"
            sh "docker ps"
        }
    }
    }    
    
    }
}
