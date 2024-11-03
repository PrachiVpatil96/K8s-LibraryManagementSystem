# Library Management System 

## SelfHosted Agent (k8s Installation)



---

# Kubernetes Cluster Setup Steps

This document outlines the high-level steps to set up a Kubernetes cluster using `kubeadm`, Docker, `cri-dockerd`, and the Flannel CNI plugin.

---

### Steps

1. **Install Docker on All Nodes**

   - Install Docker to act as the container runtime for Kubernetes on virtual machine ubuntu 22.04.
   ```bash
   curl -fsSL https://get.docker.com -o install-docker.sh
   sh install-docker.sh
   ```

2. **Install `cri-dockerd` on All Nodes**
    - Install `cri-dockerd` to enable Kubernetes to use Docker as the container runtime interface.
    ```bash
    cd /tmp
    wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.15/cri-dockerd_0.3.15.3-0.ubuntu-jammy_amd64.deb
    sudo dpkg -i cri-dockerd_0.3.15.3-0.ubuntu-jammy_amd64.deb
    sudo usermod -aG docker ubuntu
    ```
   

3. **Install `kubeadm` and `kubectl` and `kubelet`on All Nodes**
    - Install Kubernetes components `kubeadm` and `kubectl` to initialize and manage the cluster.
    ```bash
    sudo apt-get update
    # apt-transport-https may be a dummy package; if so, you can skip that package
    sudo apt-get install -y apt-transport-https ca-certificates curl gpg
    curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
    echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl
    sudo systemctl enable --now kubelet
    ```
    

4. **Initialize the Cluster on the Master Node**

   - Run `kubeadm init` on the master node to initialize the Kubernetes control plane.
   - This command provides a `join` command with a token, which is used to add worker nodes to the cluster.
   ```kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket "unix:///var/run/cri-dockerd.sock"
   ```


5. **Configure `kubectl` on the Master Node**

   - Set up `kubectl` on the master node to interact with the Kubernetes cluster.
   ```
   kubeadm join 172.31.36.196:6443 --token x0p92n.hkwq7i7kxy9dqmeb         --discovery-token-ca-cert-hash sha256:a14ab76240ffa0200efd389251b319dcf923fc4fff2221580e8b454b76e685c7 --cri-socket "unix:///var/run/cri-dockerd.sock"
   ```

6. **Install CNI Plugins for Pod Networking**

   - Kubernetes requires CNI (Container Network Interface) plugins to manage networking between pods.

7. **Install Flannel CNI Plugin**

   - Install Flannel as the CNI plugin for pod networking across the cluster.
   ```
   kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml
   ```



