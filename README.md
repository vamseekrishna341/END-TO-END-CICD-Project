1.jenkins installation commands:

First, install Jenkins on your Linux system:
sudo yum update -y
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade -y
sudo yum install java-17-amazon-corretto -y
sudo yum install jenkins -y (2.504.2-1.1)
sudo systemctl enable jenkins
sudo systemctl start jenkins

2.docker installation commands
sudo yum install docker -y   (25.0.8-1)
sudo systemctl start docker
sudo systemctl status docker
sudo usermod -aG docker jenkins
sudo usermod -aG docker ec2-user
sudo systemctl restart docker
sudo chmod 666 /var/run/docker.sock

3. aws configure

4.kubernetes installation:
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client      (Client Version: v1.33.1     Kustomize Version: v5.6.0)      
(if you cant get kubectl version perfom this commands   kubectl version --client
echo 'export PATH=$PATH:/usr/local/bin' >> ~/.bashrc
 source ~/.bashrc
 ls -l /usr/local/bin/kubectl
 hash -r
 which kubectl
 kubectl version --client]

5.eksctl installation
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version       (0.208.0)

6 install pluggins in jenkins

7. create eks cluster
eksctl create cluster \
  --name vkcluster \
  --version 1.29 \
  --region us-east-2 \
  --vpc-public-subnets subnet-0396f97e134fdb185,subnet-061b6849034dd3804 \
  --nodegroup-name krishna \
  --node-type t3.micro \
  --nodes 2 \
  --managed \
  --ssh-access \
  --ssh-public-key newkeypair

if you get an issue like older version of aws cli you will face problem while fetching kubectl get nodes it wont show so remove older version of awscli and update it to aws cli 2 and after that follow this commands :
ls -l /usr/local/bin/aws
echo $PATH
export PATH=/usr/local/bin:$PATH
echo 'export PATH=/usr/local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
aws --version
aws eks --region us-east-2 update-kubeconfig --name vkcluster
kubectl get nodes
eksctl delete cluster --name vkcluster


8. docker run -itd --name sonar -p 9000:9000 sonarqube      /by using docker 

argocd installation:
curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd
argocd version
argocd version --client
kubectl create ns argocd
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server
kubectl get all ns
kubectl -n argocd get secret argocd-initial-admin-secret
kubectl get deployments -n argocd
kubectl get pods -n argocd
kubectl get svc -n argocd
