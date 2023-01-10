pipeline {
    agent {
           label 'tanzu-mgmt'
    }
    stages {
       stage("Get newly pushed image") {
            steps {
                echo "Hello World"
                echo "New Image ID:"
                echo "${ImageSHA}"
                echo "Updating Image..."
                sh '''yq eval '".spec.template.spec.containers[0].image" = "tbsregistry.tanzu.lab/tbs-project/spring-images/pet-clinic@${ImageSHA}"' ./kustomize/deployment.yaml'''
                sh 'cat ./kustomize/deployment.yaml'
                }
           }
       stage("Push to Gitops Repo"){
               steps{
                   sh 'git clone https://github.com/i386kernel/kube-manifests.git'
                   sh 'cp ./kustomize/deployment.yaml kube-manifests/kustomize/'
                   sh 'cd kube-manifests'
                   sh 'git add .'
                   sh 'git commit -m "updated app image to newer version"'
                   sh 'git push'
               }
    }
 }
}
