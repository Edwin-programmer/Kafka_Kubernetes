# Kafka_Kubernetes
Kafka deployment on Kubernetes

## Configuration
### Docker Runtime Environment
```shell
$sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce
sudo docker run hello-world
```
### Kubectl
Configure kubectl using native package management. More details on setup are at https://kubernetes.io/docs/tasks/tools/
```
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2 curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
### Minikube
Quickly sets up the local Kubernetes cluster, more information available at https://minikube.sigs.k8s.io/docs/start/
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
sudo -E minikube start --driver=none
sudo minikube start
sudo minikube kubectl -- get po -A
sudo apt-get install -y kubelet kubeadm kubectl
sudo kubectl get pods
sudo kubectl get nodes
```
### IP tables (Skip)
Conntrack module provides stateful packet inspection for iptables
Further information is located at http://conntrack-tools.netfilter.org/
```
sudo apt-get update -y
sudo apt-get install -y conntrack
```
Enable IP tables
```
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```
### Helm Chart for Operator setup
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm version
```
Installing the chart
```
helm repo add strimzi https://strimzi.io/charts/
helm install my-release strimzi/strimzi-kafka-operator
sudo kubectl get po -w
```

### Deployments
```
sudo kubectl apply -f https://strimzi.io/examples/latest/kafka/kafka-persistent-single.yaml
sudo kubectl get svc
sudo kubectl get pods --all-namespaces -o wide
sudo kubectl describe po my-cluster-kafka-0

```
test expanding cluster...increase replica/ Kafka connections to external 
### Prometheus (JMX exporter for metrics)
WIP

