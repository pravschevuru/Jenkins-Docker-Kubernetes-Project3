pipeline {
    agent any
		
	environment {
		PROJECT_ID = 'pragiti-demo-hy'
        CLUSTER_NAME = 'hyvision-gke-cluster'
        LOCATION = 'asia-southeast1-b'
        CREDENTIALS_ID = 'jenkins-gcr'		
	}
	
    stages {
    stage("Checkout code") {
            steps {
                git credentialsId: 'BitBucket_credentials', url: 'https://chevurupravallika@bitbucket.org/pragiti-git/devops.git'
               dir("sap-commerce") {
               sh "pwd"
               sh "ls -l"
            }
            }
            }
	 /*   stage('Build Docker Image') {
		    steps {
			    sh 'whoami'
			    script {
				    myimage = docker.build("pragiti-demo-hy/devops:${env.BUILD_ID}")
			    }
		    }
	    }
	    
	    stage("Push Docker Image") {
		    steps {
			    script {
				    echo "Push Docker Image"
				    docker.withRegistry('https://gcr.io','gcr:jenkins-gcr') {
				        myimage.push("${env.BUILD_ID}")
				    }
			    }
		    }
	    }   */
	    
	    stage('Deploy to GKE Cluster') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
          dir("sap-commerce"){
	 echo "Start deployment of hybris-stateful-backoffice.yaml"
		step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'hybris-stateful-backoffice.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
	echo "Start deployment of hybris-service-bo.yaml"
		step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'hybris-service-lb-bo.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
    echo "Start deployment of hybris-stateful-hac.yaml"
		step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'hybris-stateful-hac.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
	echo "Start deployment of hybris-service-hac.yaml"
		step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'hybris-service-lb-hac.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])   
	 echo "Deployment Finished ..."
		    }
	    }
    }
    }
}
