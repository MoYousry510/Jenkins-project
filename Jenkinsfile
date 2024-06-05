@Library('Jenkins-Shared-Library')_
pipeline {
    agent any
    
    environment {
        dockerHubCredentialsID	    = 'DockerHub'  		    			// DockerHub credentials ID.
        imageName   		    = 'engyousry/ivolve-app'     			// DockerHub repo/image name.
	openshiftCredentialsID	    = 'openshiftCredentialsID'		    			// service account token credentials ID
	openshiftClusterURL	    = 'https://api.ocp-training.ivolve-test.com:6443'   // OpenShift Cluser URL.
        openshifProject 	    = 'mohamedyousry'			     		// OpenShift project name.
	    
    }
     stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                	// Navigate to the directory contains Dockerfile
                 	dir('App') {
                 		buildandPushDockerImage("${dockerHubCredentialsID}", "${imageName}")
                        
                    	}
                }
            }
        }

        stage('Deploy on OpenShift Cluster') {
            steps {
                script { 
                	dir('OpenShift') {
				deployOnOpenShift("${openshiftCredentialsID}", "${openshiftCluster}", "${openshifProject}", "${imageName}")
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
