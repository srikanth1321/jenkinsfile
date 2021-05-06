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

                    git 'https://github.com/srikanthrdy/jenkinsfile.git';

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
        
        
    }
}
