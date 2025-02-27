@Library('Jenkins-Shared-Library')_
pipeline {
    agent {
        label 'EC2'
    }
    
    environment {
        dockerHubCredentialsID	    = 'DockerHub'  		    			// DockerHub credentials ID.
        imageName   		    = 'engyousry/ivolve-app'     			// DockerHub repo/image name.
	openshiftCredentialsID	    = 'openshiftCredentialsID'		    		// service account token credentials ID
	openshiftClusterURL	    = 'https://api.ocp-training.ivolve-test.com:6443'   // OpenShift Cluser URL.
        openshiftProject 	    = 'mohamedyousry'			     		// OpenShift project name.

    }
     stages {
        stage('Build Docker Image') {
            steps {
                script {
                	// Navigate to the directory contains Dockerfile
                 	dir('app') {
                 		buildDockerImage("${imageName}")
                    	}
                }
            }
        }
        stage('push Docker Image') {
            steps {
                script {
                	// Navigate to the directory contains Dockerfile
                 	dir('app') {
                 		pushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                    	}
                }
            }
        }


        stage('Deploy on OpenShift Cluster') {
            steps {
                script { 
                	dir('OpenShift') {
				deployOnOpenShift("${openshiftCredentialsID}", "${openshiftClusterURL}", "${openshiftProject}", "${imageName}")
                    	}
                }
            }
        }
    }

    post {
        success {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline succeeded"
        }
        failure {
            echo "${JOB_NAME}-${BUILD_NUMBER} pipeline failed"
        }
    }
}
