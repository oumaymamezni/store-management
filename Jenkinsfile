pipeline {
    agent any
    environment {
        DOCKER_CREDS = credentials('docker_user_pw')
    }
    stages {
        stage('build') {
            steps {
                bat 'mvn package'
            }
        }
        stage('build docker image') {
            steps {
                bat 'docker build -t sm:jenkins-built .'
            }
        }
/*        stage('push to registry'){
            steps{
                bat " docker login --username ${DOCKER_CREDS_USR} --password ${DOCKER_CREDS_PSW}"
                bat "docker tag sm:jenkins-built ${DOCKER_CREDS_USR}/sm:${env.buldNumber}"
                bat "docker push ${DOCKER_CREDS_USR}/sm:jenkins-built"
            }

        }*/
        stage('deploy') {
            steps {
                bat 'docker-compose -p store-man up -d --force-recreate'
/*                timeout(1) {
                retry(3) {
                    bat 'docker-compose -p store-man up -d --force-recreate'
    bat 'curl localhost:8088/store-management/api/medications'
              }
            }*/
            }
        }
    }
    post {
       /* always {
            junit 'target/*reports/*.xml'
            emailext attachLog: true, body: 'test success', subject: 'test notification', to: 'hablous2@gmail.com'
        }*/
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
