# kubernetes installation 

## Installation Instractions

### Ubuntu Installation
#### Installation Instractions
1. Installing Docker 
   First step is to install **docker** on every node. This include both master and slave nodes.
   ```bash
   $ sudo apt install docker.io -y
   ```
   Ensure that docker service is enable at boot
   ```bash
   $ sudo systemctl enable docker
   ```
2. Install Kubernetes
   after installing **docker** we are ready to install kubernetes, Execute the below command on all nodes (master & slave) to install Kubernetes: 
   - installing extra packages
     ```bash
     $ apt-get update && apt-get install -y apt-transport-https curl
     ```
   - Add Kubernetes signing key:
     ```bash
     $ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
     ```
   - Add Kubernetes repository and install Kubernetes: 
     ```bash
     cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
     deb https://apt.kubernetes.io/ kubernetes-xenial main
     EOF
     ```
   - install kubernetes 
     ```bash
     apt-get update
     apt-get install -y kubelet kubeadm kubectl
     apt-mark hold kubelet kubeadm kubectl
     ```
   - Restarting the **kubelet** service
     ```bash
     systemctl daemon-reload
     systemctl restart kubelet
     ```
### Initialize Kubernetes Cluster Server
- Swap disabled. You **MUST** disable swap in order for the kubelet to work properly

#### Creating a single master cluster with kubeadm
- https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/
- https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl
1. Initializing your master
    1. Choose a pod network add-on with flag **--pod-network-cidr**
    2. (Optinal) Unless you did not Choosed **pod network**, **kubeadm** uses the network interface associated with the default **GW** to advertise the mastes IP,
       To use a different network interface specify the **--apiserver-advertise-address=<ip-address>** argument to **kubeadm init**

    3. (Optional) Run **kubeadm config images pull** prior to **kubeadm init** to verify connectivity to gcr.io registries.
    4. Set kernel value if you have choosed network type **Flannel**,**Kube-router**,**Romana**,**Weave Net**
       ```bash
       sysctl net.bridge.bridge-nf-call-iptables=1
       cat /proc/sys/net/bridge/bridge-nf-call-iptables
       ``` 
   
   Now execute **kubeadm** command
   ```bash
   kubeadm init <args> 
   ```
   Output example
   ```bash
   root@kubemaster01:~/kubernetes# kubeadm init --pod-network-cidr=10.244.0.0/16
   [init] using Kubernetes version: v1.12.2
   [preflight] running pre-flight checks
   [preflight/images] Pulling images required for setting up a Kubernetes cluster
   [preflight/images] This might take a minute or two, depending on the speed of your internet connection
   [preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
   [kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
   [kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
   [preflight] Activating the kubelet service
   [certificates] Generated ca certificate and key.
   [certificates] Generated apiserver certificate and key.
   [certificates] apiserver serving cert is signed for DNS names [kubemaster01 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs    [10.96.0.1 192.168.88.238]
   [certificates] Generated apiserver-kubelet-client certificate and key.
   [certificates] Generated etcd/ca certificate and key.
   [certificates] Generated etcd/healthcheck-client certificate and key.
   [certificates] Generated apiserver-etcd-client certificate and key.
   [certificates] Generated etcd/server certificate and key.
   [certificates] etcd/server serving cert is signed for DNS names [kubemaster01 localhost] and IPs [127.0.0.1 ::1]
   [certificates] Generated etcd/peer certificate and key.
   [certificates] etcd/peer serving cert is signed for DNS names [kubemaster01 localhost] and IPs [192.168.88.238 127.0.0.1 ::1]
   [certificates] Generated front-proxy-ca certificate and key.
   [certificates] Generated front-proxy-client certificate and key.
   [certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
   [certificates] Generated sa key and public key.
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
   [kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
   [controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
   [controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
   [controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
   [etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
   [init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests"
   [init] this might take a minute or longer if the control plane images have to be pulled
   [apiclient] All control plane components are healthy after 36.002671 seconds
   [uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
   [kubelet] Creating a ConfigMap "kubelet-config-1.12" in namespace kube-system with the configuration for the kubelets in the cluster
   [markmaster] Marking the node kubemaster01 as master by adding the label "node-role.kubernetes.io/master=''"
   [markmaster] Marking the node kubemaster01 as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
   [patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "kubemaster01" as an annotation
   [bootstraptoken] using token: sgbcb6.ip1d11duxv70uocm
   [bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
   [bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
   [bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
   [bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
   [addons] Applied essential addon: CoreDNS
   [addons] Applied essential addon: kube-proxy
   
   Your Kubernetes master has initialized successfully!
   
   To start using your cluster, you need to run the following as a regular user:
   
     mkdir -p $HOME/.kube
     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     sudo chown $(id -u):$(id -g) $HOME/.kube/config
   
   You should now deploy a pod network to the cluster.
   Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
     https://kubernetes.io/docs/concepts/cluster-administration/addons/
   
   You can now join any number of machines by running the following on each node
   as root:
   
     kubeadm join 192.168.88.238:6443 --token sgbcb6.ip1d11duxv70uocm --discovery-token-ca-cert-hash sha256:80f879a1bdc59da7238a8cd9a4a1f470e66a7fccfbff7c759b43ec32be35040d
   
   ```
   running kubctl with non-root user
   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```
   if you are running with **root** user you can run **export** command:
   ```bash
   export KUBECONFIG=/etc/kubernetes/admin.conf
   ```
   > Make sure that you save the record of **kubeadm join** that have been output , you will need it to **join node to the cluster**

2. adding nodes to cluster
3. change to non-priviliges user 
4. troubleshooting




#### Creating Highly Available Clusters with kubeadm

- related links : https://github.com/cookeem/kubeadm-ha
- https://kubernetes.io/docs/tasks/administer-cluster/highly-available-master/#starting-an-ha-compatible-cluster
- https://kubernetes.io/docs/setup/independent/high-availability/#first-steps-for-both-methods

prerequisites
- Linux version: Ubuntu 18.04 LTS
- Kernel version: 4.15.0-38-generic
  ```bash
  root@kubemaster01:~# lsb_release -a
  No LSB modules are available.
  Distributor ID: Ubuntu
  Description:    Ubuntu 18.04.1 LTS
  Release:        18.04
  Codename:       bionic
  #
  root@kubemaster01:~# uname -r
  4.15.0-38-generic
  ```
- docker version: 18.06.1-ce
  ```bash
  ~$ docker version
  Client:
   Version:           18.06.1-ce
   API version:       1.38
   Go version:        go1.10.4
   Git commit:        e68fc7a
   Built:             Fri Oct 19 19:43:14 2018
   OS/Arch:           linux/amd64
   Experimental:      false
  
  Server:
   Engine:
    Version:          18.06.1-ce
    API version:      1.38 (minimum version 1.12)
    Go version:       go1.10.4
    Git commit:       e68fc7a
    Built:            Thu Sep 27 02:39:50 2018
    OS/Arch:          linux/amd64
    Experimental:     false

  ```
- kubeadm version: v1.12.2
  ```bash
  kubeadm version: &version.Info{Major:"1", Minor:"12", GitVersion:"v1.12.2", GitCommit:"17c77c7898218073f14c8d573582e8d2313dc740", GitTreeState:"clean", BuildDate:"2018-10-24T06:51:33Z", GoVersion:"go1.10.4", Compiler:"gc", Platform:"linux/amd64"}
  ```

- kubelet version: v1.12.2
  ```bash
  $ kubelet --version
  Kubernetes v1.12.2
  ```
- required docker images
  ```bash
  root@kubemaster01:~# kubeadm config images list --kubernetes-version=$(kubelet --version| awk '{print $2}')
  k8s.gcr.io/kube-apiserver:v1.12.2
  k8s.gcr.io/kube-controller-manager:v1.12.2
  k8s.gcr.io/kube-scheduler:v1.12.2
  k8s.gcr.io/kube-proxy:v1.12.2
  k8s.gcr.io/pause:3.1
  k8s.gcr.io/etcd:3.2.24
  k8s.gcr.io/coredns:1.2.2

  # use kubeadm to pull all required docker images
  $ kubeadm config images pull --kubernetes-version=$(kubelet --version| awk '{print $2}')

  # kubernetes networks addons
  $ docker pull quay.io/calico/typha:v0.7.4
  $ docker pull quay.io/calico/node:v3.1.3
  $ docker pull quay.io/calico/cni:v3.1.3

  # kubernetes metrics server
  $ docker pull gcr.io/google_containers/metrics-server-amd64:v0.2.1
  
  # kubernetes dashboard
  $ docker pull gcr.io/google_containers/kubernetes-dashboard-amd64:v1.8.3
  
  # kubernetes heapster
  $ docker pull k8s.gcr.io/heapster-amd64:v1.5.4
  $ docker pull k8s.gcr.io/heapster-influxdb-amd64:v1.5.2
  $ docker pull k8s.gcr.io/heapster-grafana-amd64:v5.0.4
  
  # kubernetes apiserver load balancer
  $ docker pull nginx:latest
  
  # prometheus
  $ docker pull prom/prometheus:v2.3.1
  
  # traefik
  $ docker pull traefik:v1.6.3
  
  # istio
  $ docker pull docker.io/jaegertracing/all-in-one:1.5
  $ docker pull docker.io/prom/prometheus:v2.3.1
  $ docker pull docker.io/prom/statsd-exporter:v0.6.0
  $ docker pull gcr.io/istio-release/citadel:1.0.0
  $ docker pull gcr.io/istio-release/galley:1.0.0
  $ docker pull gcr.io/istio-release/grafana:1.0.0
  $ docker pull gcr.io/istio-release/mixer:1.0.0
  $ docker pull gcr.io/istio-release/pilot:1.0.0
  $ docker pull gcr.io/istio-release/proxy_init:1.0.0
  $ docker pull gcr.io/istio-release/proxyv2:1.0.0
  $ docker pull gcr.io/istio-release/servicegraph:1.0.0
  $ docker pull gcr.io/istio-release/sidecar_injector:1.0.0
  $ docker pull quay.io/coreos/hyperkube:v1.7.6_coreos.0

  ```
#### Creating a single master cluster with kubeadm
```bash

```

```bash

```

#### Deploying the Dashboard UI

```bash
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```
or 
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/alternative/kubernetes-dashboard.yaml
```
##### accessing the Dashboard UI

```bash
kubectl proxy
```