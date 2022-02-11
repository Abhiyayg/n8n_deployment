pipeline {
  environment {
    registry = "abhiya/n8n"
    registryCredential = 'dockerhub_id'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry 
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  
    stage('Deploy to Kubernetes'){
      steps{
       script {
         bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" get pods --namespace=workflow-tools'
         bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" apply -f pod.yml'
         bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" apply -f pvc.yml'
         bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" apply -f deployment.yml'
         bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" apply -f service.yml'
        // bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" port-forward pod/ tool-pod 5678:5678'
        //bat 'kubectl --kubeconfig="tf-k8s-01f25f2617-kubeconfig.yaml" port-forward pod/tool 8081:5678'    
 
         
        }
      }
    }
  }
} 
