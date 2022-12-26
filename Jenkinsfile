pipeline {
    agent any
    
    stages {
        stage('build Dockerfile') {

            steps {
                sh '''echo "FROM maven:3-alpine
                          RUN apk add --update docker openrc
                          RUN rc-update add docker boot" >/var/lib/jenkins/workspace/Dockerfile'''

            }
         }

         stage('run Dockerfile') {
             agent{
                 dockerfile {
                            filename '/var/lib/jenkins/workspace/Dockerfile'
                            args '--user root -v $HOME/.m2:/root/.m2  -v /var/run/docker.sock:/var/run/docker.sock'
                        }
             }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
}
