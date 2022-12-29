pipeline {
    agent {        
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }   
    }     
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Deploy to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'my-app', classifier: '', file: 'my-app/target/my-app.jar', type: 'jar']], credentialsId: 'Nexus', groupId: 'com.mycompany.app', nexusUrl: '10.0.0.4:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
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
    }
     post { 
        always { 
            echo 'I will always say Hello!'
              mail to: 'pl.shivashankar@gmail.com',
            subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
            body: "Something is wrong with ${env.BUILD_URL}"
        }
        aborted {
            echo 'I was aborted'
              mail to: 'pl.shivashankar@gmail.com',
            subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
            body: "Something is wrong with ${env.BUILD_URL}"
        }
        failure {
            mail to: 'pl.shivashankar@gmail.com',
            subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
            body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
