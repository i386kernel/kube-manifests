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
                sh 'yq ./kustomize/deployment.yaml "spec.template.spec.containers[0].image" = "tbsregistry.tanzu.lab/tbs-project/spring-images/${ImageSHA}"'
                sh 'cat ../kustomize/deployment.yaml'
                }
           }
       stage("Push to Gitops Repo"){
               steps{
                   sh 'git add .'
                   sh 'git commit -m "updated app image to newer version"'
                   sh 'git push'
               }
    }
 }
}