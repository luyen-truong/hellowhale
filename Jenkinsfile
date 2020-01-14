pipeline {
    agent any
    environment {
        PROJECT_ID = 'gcpcloudtest'
        CLUSTER_NAME = 'kubernetes'
        LOCATION = 'europe-west2-a'
        CREDENTIALS_ID = 'gke-6868'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
				sh 'echo $BRANCH_NAME'
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("gcr.io/gcpcloudtest/hello:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                    sh "docker push -- gcr.io/gcpcloudtest/hello:${env.BUILD_ID}"
                    }
                }
        stage('test3') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'origin/master') {
                        echo 'I only execute on the master branch'
                    } else {
                        echo 'I execute elsewhere'
                    }
                }
            }
        }
    }
}

