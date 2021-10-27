# pegkubernetes
peg kubernetes

1. Check minimum requirments:

Memory: 2 GiB or more of RAM per machine
CPUs: At least 2 CPUs on the control plane machine.

and Update Ubuntu 20.04:

sudo apt update && sudo apt -y upgrade && sudo systemctl reboot

2. Install kubelet, kubeadm and kubectl:

sudo apt -y install curl apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update 
sudo apt -y install vim git curl wget kubelet kubeadm kubectl && sudo apt-mark hold kubelet kubeadm kubectl

kubectl version --client && kubeadm version

sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab && sudo swapoff -a


3.
# Enable kernel modules
sudo modprobe overlay && sudo modprobe br_netfilter

# Add some settings to sysctl
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

# Reload sysctl
sudo sysctl --system
                                            
4. Installing Docker runtime:
                                            
# Add repo and Install packages
sudo apt update && sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update && sudo apt install -y containerd.io docker-ce docker-ce-cli

# Create required directories
sudo mkdir -p /etc/systemd/system/docker.service.d

# Create daemon json config file
sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

# Start and enable Services
sudo systemctl daemon-reload && sudo systemctl restart docker && sudo systemctl enable docker
                                      
5. Initialize master node

Login to the server to be used as master and make sure that the br_netfilter module is loaded:

pegaz@peg01:~$ lsmod | grep br_netfilter
br_netfilter           28672  0
bridge                249856  1 br_netfilter

sudo systemctl enable kubelet
                                      
                                      
                                      
                                      
