pipeline {
    agent any
    environment {
        GCP_PROJECT_ID = 'pragiti-demo-hy'
         GKE_CLUSTER_NAME = 'hyvision-gke-cluster'
        GCP_ZONE = 'asia-southeast1-b'
        CREDENTIALS_ID = 'jenkins-gcr'
    }
   stages {
        stage("Checkout code") {
           steps {
                  git credentialsId: 'github_crede', url: 'https://github.com/pravschevuru/Jenkins-Docker-Kubernetes-Project3.git'
            }
        }
        stage("Build image") {
            steps {
                script {
                    app = docker.build("pragiti-demo-hy/test:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://gcr.io','gcr:jenkins-gcr'){
                    app.push("${env.BUILD_ID}")
                    app.push("latest")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
               sh "sed -i 's/latest/${env.BUILD_ID}/g' deployment.yaml"
			    echo "Start deployment of deployment.yaml"
			    step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
    }    
}
}
}
