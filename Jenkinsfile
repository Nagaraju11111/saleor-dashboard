pipeline {
    agent { label 'NODE' }
    triggers { pollSCM ('* * * * *') }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/Nagaraju11111/saleor-dashboard.git',
                    branch: 'main'
            }
        }
        stage ('dockerimagebuild') {
            steps {
               sh 'docker image build -t sdb:1.0 .'
               sh 'docker image tag sdb:1.0 9052171017/sdb:1.0'
               sh 'docker image push 9052171017/sdb:1.0'
            }
        }
        stage ('terraform') {
            agent { label 'NODE2' }
            steps {
               git url: 'https://github.com/Nagaraju11111/terraform.git', branch: 'master'
               sh 'terraform init'
               sh 'terraform validate'
               sh 'terraform apply -var-file="dev.tfvars" -auto-approve'
               sh 'az aks get-credentials --resource-group terraform --name akscluster'
               sh 'kubectl get nodes'
            }
        }
    }
}