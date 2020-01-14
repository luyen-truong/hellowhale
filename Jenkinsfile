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
        stage('Deploy to GKE') {
			if (env.BRANCH_NAME == 'master') {
            steps{
                sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
				}
			}
		}
	}
}

