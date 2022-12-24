pipeline{
    agent { label 'kubectl' }
    stages{
        stage('vcs'){
            steps{
                git branch:"main", url:'https://github.com/krishna5683akp/learn-terraform-provision-eks-cluster.git'

            }
        }
        stage('cluster creation') {
            steps{
                sh """terraform init
                      terraform apply -auto-approve
                      curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.15/2022-10-31/bin/linux/amd64/kubectl
                      curl -o kubectl.sha256 https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.15/2022-10-31/bin/linux/amd64/kubectl.sha256
                      chmod +x ./kubectl
                      mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
                      echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
                      kubectl version --short --client
                      aws eks --region $(terraform output -raw region) update-kubeconfig \
                      --name $(terraform output -raw cluster_name)
                      kubectl cluster-info
                      kubectl get nodes """
            }
        }
    }
}