# cluster-api-aws-demo

[![Codacy Badge](https://app.codacy.com/project/badge/Grade/5f360c2ece774efe8565c3dc0164d98b)](https://app.codacy.com/gh/afonsoaugusto/cluster-api-aws-demo/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade)

<https://cluster-api-aws.sigs.k8s.io/getting-started.html>

```sh
vagrant ssh
cd /opt

cat > kind-cluster-with-extramounts.yaml <<EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraMounts:
    - hostPath: /var/run/docker.sock
      containerPath: /var/run/docker.sock
EOF

kind create cluster --config kind-cluster-with-extramounts.yaml

kubectl cluster-info --context kind-kind
Kubernetes control plane is running at https://127.0.0.1:40251
CoreDNS is running at https://127.0.0.1:40251/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

alias k=kubectl

curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/v1.4.2/clusterctl-linux-amd64 -o clusterctl
sudo install -o root -g root -m 0755 clusterctl /usr/local/bin/clusterctl
clusterctl version

 clusterctl version
clusterctl version: &version.Info{Major:"1", Minor:"4", GitVersion:"v1.4.2", GitCommit:"7b92ce4577e36adf013dc1dfd2b239fde4c36bc4", GitTreeState:"clean", BuildDate:"2023-05-02T15:22:41Z", GoVersion:"go1.19.6", Compiler:"gc", Platform:"linux/amd64"}


# Enable the experimental Cluster topology feature.
export CLUSTER_TOPOLOGY=true

# Initialize the management cluster
clusterctl init --infrastructure docker




vagrant@vagrant:/opt$ clusterctl init --infrastructure docker
Fetching providers
Installing cert-manager Version="v1.11.1"
Waiting for cert-manager to be available...


Error: timed out waiting for the condition
vagrant@vagrant:/opt$ 
vagrant@vagrant:/opt$
vagrant@vagrant:/opt$ clusterctl init --infrastructure docker
Fetching providers
Skipping installing cert-manager as it is already installed
Installing Provider="cluster-api" Version="v1.4.2" TargetNamespace="capi-system"
Installing Provider="bootstrap-kubeadm" Version="v1.4.2" TargetNamespace="capi-kubeadm-bootstrap-system"
Installing Provider="control-plane-kubeadm" Version="v1.4.2" TargetNamespace="capi-kubeadm-control-plane-system"
Installing Provider="infrastructure-docker" Version="v1.4.2" TargetNamespace="capd-system"

Your management cluster has been initialized successfully!

You can now create your first workload cluster by running the following:

  clusterctl generate cluster [name] --kubernetes-version [version] | kubectl apply -f -




k get all -A
NAMESPACE                           NAME                                                                READY   STATUS    RESTARTS      AGE
capd-system                         pod/capd-controller-manager-c479754b7-v9wzf                         1/1     Running   0             2m7s
capi-kubeadm-bootstrap-system       pod/capi-kubeadm-bootstrap-controller-manager-bcdfbf4c5-mcjw2       1/1     Running   0             2m15s
capi-kubeadm-control-plane-system   pod/capi-kubeadm-control-plane-controller-manager-b9485b857-klsmt   1/1     Running   0             2m10s
capi-system                         pod/capi-controller-manager-9d9548dc8-62hjm                         1/1     Running   0             2m18s
cert-manager                        pod/cert-manager-698dc8bdd5-44bcr                                   1/1     Running   0             12m
cert-manager                        pod/cert-manager-cainjector-6db899f596-zf9rk                        1/1     Running   0             12m
cert-manager                        pod/cert-manager-webhook-78489cc458-wpgf7                           1/1     Running   0             12m
kube-system                         pod/coredns-5d78c9869d-9zjwt                                        1/1     Running   0             16m
kube-system                         pod/coredns-5d78c9869d-zxm6w                                        1/1     Running   0             16m
kube-system                         pod/etcd-kind-control-plane                                         1/1     Running   0             16m
kube-system                         pod/kindnet-qs4sh                                                   1/1     Running   0             16m
kube-system                         pod/kube-apiserver-kind-control-plane                               1/1     Running   0             16m
kube-system                         pod/kube-controller-manager-kind-control-plane                      1/1     Running   2 (15m ago)   16m
kube-system                         pod/kube-proxy-mtvdw                                                1/1     Running   0             16m
kube-system                         pod/kube-scheduler-kind-control-plane                               1/1     Running   2 (15m ago)   16m
local-path-storage                  pod/local-path-provisioner-59854d8c69-cn9fz                         1/1     Running   0             16m

NAMESPACE                           NAME                                                 TYPE        CLUSTER-IP   
  EXTERNAL-IP   PORT(S)                  AGE
capd-system                         service/capd-webhook-service                         ClusterIP   10.96.61.138 
  <none>        443/TCP                  2m7s
capi-kubeadm-bootstrap-system       service/capi-kubeadm-bootstrap-webhook-service       ClusterIP   10.96.100.18 
  <none>        443/TCP                  2m15s
capi-kubeadm-control-plane-system   service/capi-kubeadm-control-plane-webhook-service   ClusterIP   10.96.246.36 
  <none>        443/TCP                  2m11s
capi-system                         service/capi-webhook-service                         ClusterIP   10.96.104.7  
  <none>        443/TCP                  2m18s
cert-manager                        service/cert-manager                                 ClusterIP   10.96.226.87 
  <none>        9402/TCP                 12m
cert-manager                        service/cert-manager-webhook                         ClusterIP   10.96.126.47 
  <none>        443/TCP                  12m
default                             service/kubernetes                                   ClusterIP   10.96.0.1    
  <none>        443/TCP                  16m
kube-system                         service/kube-dns                                     ClusterIP   10.96.0.10   
  <none>        53/UDP,53/TCP,9153/TCP   16m

NAMESPACE     NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR      
      AGE
kube-system   daemonset.apps/kindnet      1         1         1       1            1           kubernetes.io/os=linux   16m
kube-system   daemonset.apps/kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   16m

NAMESPACE                           NAME                                                            READY   UP-TO-DATE   AVAILABLE   AGE
capd-system                         deployment.apps/capd-controller-manager                         1/1     1     
       1           2m7s
capi-kubeadm-bootstrap-system       deployment.apps/capi-kubeadm-bootstrap-controller-manager       1/1     1     
       1           2m15s
capi-kubeadm-control-plane-system   deployment.apps/capi-kubeadm-control-plane-controller-manager   1/1     1     
       1           2m10s
capi-system                         deployment.apps/capi-controller-manager                         1/1     1     
       1           2m18s
cert-manager                        deployment.apps/cert-manager                                    1/1     1     
       1           12m
cert-manager                        deployment.apps/cert-manager-cainjector                         1/1     1     
       1           12m
cert-manager                        deployment.apps/cert-manager-webhook                            1/1     1     
       1           12m
kube-system                         deployment.apps/coredns                                         2/2     2     
       2           16m
local-path-storage                  deployment.apps/local-path-provisioner                          1/1     1     
       1           16m

NAMESPACE                           NAME                                                                      DESIRED   CURRENT   READY   AGE
capd-system                         replicaset.apps/capd-controller-manager-c479754b7                         1   
      1         1       2m7s
capi-kubeadm-bootstrap-system       replicaset.apps/capi-kubeadm-bootstrap-controller-manager-bcdfbf4c5       1   
      1         1       2m15s
capi-kubeadm-control-plane-system   replicaset.apps/capi-kubeadm-control-plane-controller-manager-b9485b857   1   
      1         1       2m10s
capi-system                         replicaset.apps/capi-controller-manager-9d9548dc8                         1   
      1         1       2m18s
cert-manager                        replicaset.apps/cert-manager-698dc8bdd5                                   1   
      1         1       12m
cert-manager                        replicaset.apps/cert-manager-cainjector-6db899f596                        1   
      1         1       12m
cert-manager                        replicaset.apps/cert-manager-webhook-78489cc458                           1   
      1         1       12m
kube-system                         replicaset.apps/coredns-5d78c9869d                                        2   
      2         2       16m
local-path-storage                  replicaset.apps/local-path-provisioner-59854d8c69                         1   
      1         1       16m


# The list of service CIDR, default ["10.128.0.0/12"]
export SERVICE_CIDR=["10.96.0.0/12"]

# The list of pod CIDR, default ["192.168.0.0/16"]
export POD_CIDR=["192.168.0.0/16"]

# The service domain, default "cluster.local"
export SERVICE_DOMAIN="k8s.test"


clusterctl generate cluster capi-quickstart --flavor development \
  --kubernetes-version v1.27.1 \
  --control-plane-machine-count=1 \
  --worker-machine-count=1 \
  > capi-quickstart.yaml


kubectl apply -f capi-quickstart.yaml


kubectl apply -f capi-quickstart.yaml

clusterclass.cluster.x-k8s.io/quick-start created
dockerclustertemplate.infrastructure.cluster.x-k8s.io/quick-start-cluster created
kubeadmcontrolplanetemplate.controlplane.cluster.x-k8s.io/quick-start-control-plane created
dockermachinetemplate.infrastructure.cluster.x-k8s.io/quick-start-control-plane created
dockermachinetemplate.infrastructure.cluster.x-k8s.io/quick-start-default-worker-machinetemplate created
kubeadmconfigtemplate.bootstrap.cluster.x-k8s.io/quick-start-default-worker-bootstraptemplate created
cluster.cluster.x-k8s.io/capi-quickstart created

kubectl get cluster
NAME              PHASE          AGE   VERSION
capi-quickstart   Provisioning   18s   v1.27.1




clusterctl describe cluster capi-quickstart

NAME                                                           READY  SEVERITY  REASON                           SINCE  MESSAGE
Cluster/capi-quickstart                                        False  Warning   ScalingUp                        2m50s  Scaling up control plane to 1 replicas (actual 0)
├─ClusterInfrastructure - DockerCluster/capi-quickstart-m6n9w  True                                              2m29s
├─ControlPlane - KubeadmControlPlane/capi-quickstart-zh2dw     False  Warning   ScalingUp                        2m50s  Scaling up control plane to 1 replicas (actual 0)
│ └─Machine/capi-quickstart-zh2dw-lvk8s                        False  Warning   BootstrapFailed                  64s    1 of 2 completed
└─Workers

  └─MachineDeployment/capi-quickstart-md-0-bcfnx               False  Warning   WaitingForAvailableMachines      2m50s  Minimum availability requires 1 replicas, current 0 available
    └─Machine/capi-quickstart-md-0-bcfnx-f5db569bxw6bgf-w6zjk  False  Info      WaitingForControlPlaneAvailable  2m28s  0 of 2 completed
vagrant@vagrant:/opt$ 
vagrant@vagrant:/opt$  kubectl get cluster
NAME              PHASE         AGE     VERSION
capi-quickstart   Provisioned   2m55s   v1.27.1
vagrant@vagrant:/opt$ k describe cluster capi-quickstart.yaml 
Error from server (NotFound): clusters.cluster.x-k8s.io "capi-quickstart.yaml" not found
vagrant@vagrant:/opt$ k describe cluster capi-quickstart
Name:         capi-quickstart
Namespace:    default
Labels:       cluster.x-k8s.io/cluster-name=capi-quickstart
              topology.cluster.x-k8s.io/owned=
Annotations:  <none>
API Version:  cluster.x-k8s.io/v1beta1
Kind:         Cluster
Metadata:
  Creation Timestamp:  2023-05-05T21:09:35Z
  Finalizers:
    cluster.cluster.x-k8s.io
  Generation:        3
  Resource Version:  2261
  UID:               aaad0f8f-d17b-43dc-af27-6abbb4dbf1b6
Spec:
  Cluster Network:
    Pods:
      Cidr Blocks:
        192.168.0.0/16
    Service Domain:  k8s.test
    Services:
      Cidr Blocks:
        10.96.0.0/12
  Control Plane Endpoint:
    Host:  172.18.0.3
    Port:  6443
  Control Plane Ref:
    API Version:  controlplane.cluster.x-k8s.io/v1beta1
    Kind:         KubeadmControlPlane
    Name:         capi-quickstart-zh2dw
    Namespace:    default
  Infrastructure Ref:
    API Version:  infrastructure.cluster.x-k8s.io/v1beta1
    Kind:         DockerCluster
    Name:         capi-quickstart-m6n9w
    Namespace:    default
  Topology:
    Class:  quick-start
    Control Plane:
      Metadata:
      Replicas:  1
    Variables:
      Name:   imageRepository
      Value:
      Name:   etcdImageTag
      Value:
      Name:   coreDNSImageTag
      Value:
      Name:   podSecurityStandard
      Value:
        Audit:    restricted
        Enabled:  true
        Enforce:  baseline
        Warn:     restricted
    Version:      v1.27.1
    Workers:
      Machine Deployments:
        Class:  default-worker
        Metadata:
        Name:      md-0
        Replicas:  1
Status:
  Conditions:
    Last Transition Time:  2023-05-05T21:09:37Z
    Message:               Scaling up control plane to 1 replicas (actual 0)
    Reason:                ScalingUp
    Severity:              Warning
    Status:                False
    Type:                  Ready
    Last Transition Time:  2023-05-05T21:09:36Z
    Message:               Waiting for control plane provider to indicate the control plane has been initialized  
    Reason:                WaitingForControlPlaneProviderInitialized
    Severity:              Info
    Status:                False
    Type:                  ControlPlaneInitialized
    Last Transition Time:  2023-05-05T21:09:37Z
    Message:               Scaling up control plane to 1 replicas (actual 0)
    Reason:                ScalingUp
    Severity:              Warning
    Status:                False
    Type:                  ControlPlaneReady
    Last Transition Time:  2023-05-05T21:09:58Z
    Status:                True
    Type:                  InfrastructureReady
    Last Transition Time:  2023-05-05T21:11:05Z
    Status:                True
    Type:                  TopologyReconciled
  Infrastructure Ready:    true
  Observed Generation:     3
  Phase:                   Provisioned
Events:
  Type    Reason               Age                    From                Message
  ----    ------               ----                   ----                -------
  Normal  Pending              3m13s (x2 over 3m13s)  cluster-controller  Cluster capi-quickstart is Pending      
  Normal  TopologyCreate       3m13s                  topology/cluster    Created "DockerCluster/capi-quickstart-m6n9w"
  Normal  TopologyCreate       3m12s                  topology/cluster    Created "DockerMachineTemplate/capi-quickstart-control-plane-7jp2v"
  Normal  TopologyCreate       3m12s                  topology/cluster    Created "KubeadmControlPlane/capi-quickstart-zh2dw"
  Normal  TopologyUpdate       3m12s                  topology/cluster    Updated "Cluster/capi-quickstart"       
  Normal  TopologyCreate       3m12s                  topology/cluster    Created "DockerMachineTemplate/capi-quickstart-md-0-infra-bcxx5"
  Normal  TopologyCreate       3m12s                  topology/cluster    Created "KubeadmConfigTemplate/capi-quickstart-md-0-bootstrap-pj2v7"
  Normal  Provisioning         3m12s                  cluster-controller  Cluster capi-quickstart is Provisioning 
  Normal  TopologyCreate       3m11s                  topology/cluster    Created "MachineDeployment/capi-quickstart-md-0-bcfnx"
  Normal  TopologyUpdate       3m10s                  topology/cluster    Updated "KubeadmControlPlane/capi-quickstart-zh2dw"
  Normal  InfrastructureReady  2m49s (x2 over 2m50s)  cluster-controller  Cluster capi-quickstart InfrastructureReady is now true
  Normal  Provisioned          2m49s (x2 over 2m49s)  cluster-controller  Cluster capi-quickstart is Provisioned  
vagrant@vagrant:/opt$ 

kubectl get kubeadmcontrolplane
NAME                    CLUSTER           INITIALIZED   API SERVER AVAILABLE   REPLICAS   READY   UPDATED   UNAVAILABLE   AGE   VERSION
capi-quickstart-qz4xh   capi-quickstart   true                                 1                  1         1             11m   v1.27.1       
vagrant@vagrant:/opt$ kind get kubeconfig --name capi-quickstart > capi-quickstart.kubeconfig


clusterctl get kubeconfig capi-quickstart > capi-quickstart.kubeconfig
kind get kubeconfig --name capi-quickstart > capi-quickstart.kubeconfig

export KUBECONFIG=/opt/capi-quickstart.kubeconfig

 k get nodes
NAME                          STATUS     ROLES           AGE     VERSION
capi-quickstart-qz4xh-pxmzr   NotReady   control-plane   9m21s   v1.27.1


 kind get clusters
capi-quickstart
kind

kubectl --kubeconfig=./capi-quickstart.kubeconfig \
  apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.1/manifests/calico.yaml

kubectl --kubeconfig=./capi-quickstart.kubeconfig get nodes


kubectl taint nodes capi-quickstart-qz4xh-pxmzr node.kubernetes.io/not-ready-
```
