pipeline {
    agent any
    environment {
        PROJECT_ID = 'gcpcloudtest'
        CLUSTER_NAME_PRO = 'production'
		CLUSTER_NAME_STG = 'staging'
        LOCATION = 'europe-west2-a'
        CREDENTIALS_ID = 'gke-6868'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
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
		stage ('Deploy to GKE : Master') {
			when {
				expression {
				return env.GIT_BRANCH == "origin/master"
					}
				} 
			steps { 
				sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_PRO, location: env.LOCATION, manifestPattern: 'deployment.yaml',
				credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
				  }
			    }
		stage ('Deploy to GKE-Staging : DEV') {
			when {
				expression {
				return env.GIT_BRANCH == "origin/dev"
					}
				} 
			steps { 
				sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
				step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME_STG, location: env.LOCATION, manifestPattern: 'deployment.yaml',
				credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
				  }
			    }
			}
		}
