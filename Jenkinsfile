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
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("luyentv/hello:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage ('Test 3: Master') {
<<<<<<< HEAD
			when {
				expression {
				return env.GIT_BRANCH == "origin/master"
					}
				} 
			steps { 
				echo 'I only execute on the master branch.' 
				}
			}

=======
			when { branch 'master' }
			steps { 
			echo 'I only execute on the master branch.' 
				}
			}

		stage ('Test 3: Dev') {
			when { not { branch 'master' } }
			steps {
			echo 'I execute on non-master branches.'
				  }
				}
		}
	  }

	}
