# pegkubernetes
peg kubernetes

1. Check minimum requirments:

Memory: 2 GiB or more of RAM per machine
CPUs: At least 2 CPUs on the control plane machine.

and Update Ubuntu 20.04:

sudo apt update
sudo apt -y upgrade && sudo systemctl reboot

2. Install kubelet, kubeadm and kubectl:

sudo apt -y install curl apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt -y install vim git curl wget kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl


