pipeline {
    agent {
           label 'tanzu-mgmt'
    }
    environment {
        imagerepo = "tanzuharbor.tanzu.lab/app-images/pet-clinic@"
        TBS_BUILT_IMG = "tbsregistry.tanzu.lab/tbs-project/spring-images/pet-clinic:latest"
    }
    stages {
        stage("Pipeline Triggerred"){
            steps{
                echo "Pipeline Triggerred through WebHook"
            }
        }
        stage("Capture the New Image Tag"){
            steps{
                echo "Capturing ImageSHA"
                echo "${ImageSHA}"
            }
        }
        stage("Cleaning up the latent configs"){
            steps{
                sh 'ls -la'
                sh 'rm -rf kube-manifests'
                sh 'ls -la'
            }
        }
        stage ("Clone Git Repository"){
            steps{
                sh 'git clone https://github.com/i386kernel/kube-manifests.git'
                sh 'pwd'
            }
        }
        stage("Generate Kubernetes Manifests"){
            steps{
            dir ('kube-manifests'){
                sh 'kubectl create deployment se-spring-boot --image=tanzuharbor.tanzu.lab/app-images/pet-clinic@${ImageSHA} --replicas=2 --namespace=se-spring-boot-app --dry-run=client -o yaml > deployment.yaml'
                sh 'cat deployment.yaml'
                sh 'cp deployment.yaml ./kustomize/deployment.yaml'
                sh 'rm deployment.yaml'
            }
            }
        }
        stage ("Commit manifests to Git"){
            steps{
                dir ('kube-manifests'){
                sh 'ls -la'
                sh 'git add .'
                sh 'git commit -m "updated app image to newer version"'
                sh 'git push https://<username>:<secret>@github.com/i386kernel/kube-manifests.git'
            }
            }
        }
    }
}
