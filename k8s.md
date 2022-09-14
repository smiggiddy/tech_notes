[# KodeKloud CKA Prep Course](#kodekloud-cka-prep-course)  
[# Core Concepts](#core-concepts)  
[##  Cluster Architecture](#cluster-architecture)  
[## ETCD For beginners](#etcd-for-beginners)  
[## ETCD in k8](#etcd-in-k8)  
[## Kube API Server](#kube-api-server)  
[## Kube Controller manager](#kube-controller-manager)
[## Kube scheduler](#kube-scheduler)
[## kubelet](#kubelet)
[## Kube Proxy](#kube-proxy)
[## PODs](#pods)
[## ReplicaSets](#replicasets)
[## Deployments](#deployments)
[## Certification tips](#certification-tips)
[## Services](#services)
[# Service-definition.yml](#service-definition.yml)
[## Services Cluster IP](#services-cluster-ip)
[# ClusterIP Service-definition.yml](#clusterip-service-definition.yml)
[## Services Loadbalancer](#services-loadbalancer)
[# ClusterIP Service-definition.yml](#clusterip-service-definition.yml)
[## Namespaces](#namespaces)
[## Imperative vs Declarative](#imperative-vs-declarative)
[# Declarative appr](#declarative-appr)
[## Kubectl Apply cmd](#kubectl-apply-cmd)
[# Scheduling](#scheduling)
[## Manual Scheduling](#manual-scheduling)
[## Labels and Selectors](#labels-and-selectors)
[## Taints and Tolerations](#taints-and-tolerations)
[## Node Selectors](#node-selectors)
[## Node Affinity](#node-affinity)
[## Taints and Tolerations Vs Node Affinity](#taints-and-tolerations-vs-node-affinity)
[## Resource Limits](#resource-limits)
[## DaemonSets](#daemonsets)
[## Static Pods](#static-pods)
[## Multiple Schedulers](#multiple-schedulers)
[## Configuring K8s Scheduler](#configuring-k8s-scheduler)
[# Logging & Monitoring](#logging-&-monitoring)
[## Monitor Cluster Components](#monitor-cluster-components)
[## Managing Application Logs](#managing-application-logs)
[# Application Lifecycle Management](#application-lifecycle-management)
[## Rolling Updates and Rollbacks](#rolling-updates-and-rollbacks)
[## Application Configurations](#application-configurations)
[## ENV Variables in K8s](#env-variables-in-k8s)
[# other examples](#other-examples)
[## Configure configMap in Apps](#configure-configmap-in-apps)
[# single env](#single-env)
[# volumes](#volumes)
[## Secrets](#secrets)
[# inject all values as env](#inject-all-values-as-env)
[# inject single env](#inject-single-env)
[# volume method to inject secret](#volume-method-to-inject-secret)
[## Multi Container PODs](#multi-container-pods)
[## Init Containers](#init-containers)
[# Cluster Maintenance](#cluster-maintenance)
[# OS Upgrades](#os-upgrades)
[## Kubernetes Software Versions](#kubernetes-software-versions)
[## Cluster Upgrade process](#cluster-upgrade-process)
[## Backup and Restore](#backup-and-restore)
[# Security](#security)
[## K8 Security Primitives](#k8-security-primitives)
[## Authentication](#authentication)
[## TLS Basics](#tls-basics)
[## TLS in K8s](#tls-in-k8s)
[## TLS in k8s --- Certificate Creation](#tls-in-k8s-----certificate-creation)
[## View Certificate Details](#view-certificate-details)
[## Certificates API](#certificates-api)
[## Kubeconfig](#kubeconfig)
[## API Groups](#api-groups)
[## Authorization in K8s](#authorization-in-k8s)
[## RBAC](#rbac)
[## Cluster Roles](#cluster-roles)
[## Service Accounts](#service-accounts)
[## Image Security](#image-security)
[## Pre-req - Security in Docker](#pre-req---security-in-docker)
[## Container Security](#container-security)
[## Network Policies](#network-policies)
[## Developing Network Policies](#developing-network-policies)
[# Storage](#storage)
[## Intro to Docker Storage](#intro-to-docker-storage)
[## storage drivers -](#storage-drivers--)
[## volume driver plugins -](#volume-driver-plugins--)
[## Container Storage Interface](#container-storage-interface)
[## Volumes](#volumes)
[## Persistent Volumes](#persistent-volumes)
[## Persistent Volume Claims](#persistent-volume-claims)
[## Storrage Classes](#storrage-classes)
[# Networking](#networking)
[## Network Namespaces](#network-namespaces)
[## Docker Networking](#docker-networking)
[## Container Networking Interface (CNI)](#container-networking-interface-(cni))
[## Cluster Networking](#cluster-networking)
[## Pod Networking](#pod-networking)
[## CNI in k8s](#cni-in-k8s)
[## CNI weave](#cni-weave)
[## IPAM (weave)](#ipam-(weave))
[## Service Networking](#service-networking)
[## DNS in K8s](#dns-in-k8s)
[## CoreDNS in K8s](#coredns-in-k8s)
[## Ingress](#ingress)
[# Design and Install a K8s Cluster](#design-and-install-a-k8s-cluster)
[## Config HA](#config-ha)
[## ETCD in HA](#etcd-in-ha)
[## Troubleshooting k8s](#troubleshooting-k8s)
[## Worker node Failure](#worker-node-failure)
[# JSON Path In k8s](#json-path-in-k8s)
[## JSON Path query](#json-path-query)





# KodeKloud CKA Prep Course

# Core Concepts

##  Cluster Architecture 

K8 purpose is to host apps in containers in automated fashion.. with easily enabled svc between applications

Worker nodes -- host apps as containers
Control / Master node - manage plan, schedule, monitor nodes

__control plane components__

ETCD - key val holding what data for k8s



kube-apiserver: orchestrating all ops within cluster
controller-manager:
kube-scheduler -- chooses what node to place container on based on parameters
Node-controller - takes care of nodes, onboarding new nodes, etc
replication-controller - ensures desired containers are running 

container runtime engine -- needs to be installed on all nodes so they can all talk..
k8s supports containerd and rkt

kubelet - runs on all nodes.. listen for instructions.. and listens for monitors. 
Kube-proxy - ensures necessar rules are in place so that each node can reach eachother


## ETCD For beginners

ETCD - distributed reliable key-val sore that is fast

`install etcd`
download binaires, extract, run etcd service

`./etcdctl set key1 value1` # creates entry in db
`./etcdctl get key1` 

## ETCD in k8

`etcd` stores nodes, pods, configs, secrets, accounts, roles, bindings, others
`kubectl get` comes from etcd
once changes are in etcd server then changes are complete

two types of k8 deployments:
practice env deployed using kubeadm 

setup manual -
download etcd binaries. 
then config as masternode
things are passed as certs 

`--advertise-client-urls https://${INTERNAL_IP}:2370` should be default port and url for etcd service 


kubeadm -setup

`kubectl get pods -n kube-system` 

`kubectl exec etcd-maser -n kube-system etcdctl get / --prefix -keys-only` 

k8s stores data in specific strucuture

registry --- minions, pods, replicasets, etc


HA env multiple etcd instances.. 
in `--initial-cluster contorller-0=https://${CONTROLLER0_IP}:2380, controller-1=http://....`


## Kube API Server

`kubectl` requests to kubectl server

you could do 

`curl -X POST /api/v1/namespaces/default/pods ...[other]` 

kubeapi server.. ony component that interacts with etcd store 
1. authenticate user
2. validate request
3. retrieve data
4. update ETCD
5. scheduler
6. kubelet 

you can config kube-api server the hard way by downloading binary and configuring it

etcd-cafile, etcd-certfile, etcd-keyfile 
kubelet{certificate,client-certificate,client-key,https} 

--etcd-servers = where api servers are

kubeadm
`kube-apiserver-master`

`/etc/kubernetes/manisfests/kube-apiserver.yaml` 
`etc/systemd/system/kube-apiserver.service` 

`ps -aux | grep kube-apiserver`

## Kube Controller manager 

watch status
remediate situation

node-controller - uses kubeapiserver
node monitor period = 5s
node monitor grace period = 40s
POD eviction timeout = 5m 
before pods are moved out to another node to healthy nodes

replication-controller - monitoring status of replica sets.. ensures desired number of pods are in the desired replica set

kube-controller-manger -- installs different controller managers

kube-controller-manager.service

## Kube scheduler 

only for deciding which pod should go on which node
kubelet actually puts the pod where it should go


Kube-scheduler tries to find node that's best for the pod based on the cpus / memory resources

1. Filter Nodes
2. Rank Nodes - based on specs

you can write your own scheduler, etc

`/cat/kubernetes/manifests/` contains the different options

## kubelet 

register node
create pod
monitor node & pods

__kubeadm does not deploy kubelets__

manually install kubelet on worker nodes

## Kube Proxy

POD Network - internal virtual network.. all pods connect to it so they can com with eachother. 

service only lives in k8s memory

kube-proxy process that runs on each node. looks for new services, creates rules on each node to fwd traffic.. uses iptables rules 
creates iptables node on each service
10.96.0.12 

## PODs

smallest object to create in k8s

NODE 
pod    pod 

single pod can have multi-containters inside pod

`kubectl run nginx --image=nginx` to run a pod with nginx image 

to figure out what node a pod is on
use `k get pods -o wide` or look at desribe 

to create pod manifest file use 

`kubectl run --dry-run=client -o yaml`

## ReplicaSets

replication controller --- replicasets
assist with load balancing (lb) and scaling
it can assist with deploying pods across multiple nodes  

replication controller --- older tech 
replicaset --- new way to do replication 

```yaml
apiVersion: apps/v1
kind: ReplicationController
metadata: 

spec:
```

replicaset can manage pods that aren't in the replica definition. 
selector is a tag. if not defined it takes default value as labels in the file 
matchLabels: will match the lables that were defined. 


## Deployments

k8s object that allows us to do rolling updates

similar defintion to replicaset.. kind is a deployment 

`kubectl get all` will show everything 

## Certification tips
https://kubernetes.io/docs/reference/kubectl/conventions/

`kubectl create deployment --image=imageName containername --dry-run=client -o yaml` to generate a yaml file you can add --replicas=desirednumofreplicas to control replica amount

## Services 

enable comm between diff components external and internal 
allows connection with apps or users

svc are k8s objects
listens to port on a node --> fwd to port on Pod 
NodePort svc

listens to port on the node and then fwd to pod

Service Types
NodePort -- Makes internal Pod assesile on the pod
ClusterIP -- Creates virtual IP inside cluster.. allows comm between different svc
LoadBalancer -- creates a LB to distribute load 

TargetPort --- Pod Port
Service "Port" --- Service Port
Service has IP -- Cluster IP
NodePort -- Port used to access service externally 
NodePort --- 30K - 32767 port range 

```yaml

# Service-definition.yml

apiVersion: v1
kind: Service
metadata:
       name: myapp-service

spec: 
       type: NodePort
       ports:
              - targetPort: 80
                  port: 80
                  nodePort: 300008
       selector:
              app: myapp # these links service to pod
              type: front-end 
```

if no nodePort is provided one will be assigned
no port assigned it will be assumed targetport will be same as port

use labels and selectors to connect them


In prod env with HA... the service will have a random algo to loadbalance where the pods will hit


## Services Cluster IP

help group pods together and create a single interface for all the groups

```yaml

# ClusterIP Service-definition.yml

apiVersion: v1
kind: Service
metadata:
       name: backend

spec: 
       type: ClusterIP # is default type so doesn't need to be typed
       ports:
              - targetPort: 80 # Port where backend is exposed
                  port: 80 # port where the service is exposed 
       selector:
              app: myapp # these links service to pod
              type: back-end 
```

## Services Loadbalancer

HAproxy or NGINX -- are good proxys 

k8s can intergrate with cloud loadbalancers too


```yaml

# ClusterIP Service-definition.yml

apiVersion: v1
kind: Service
metadata:
       name: backend

spec: 
       type: LoadBalancer # support with AWS, GCP, etc only works with cloud providers 
       ports:
              - targetPort: 80 # Port where backend is exposed
                  port: 80 # port where the service is exposed 
                  NodePort: 30001
       selector:
              app: myapp # these links service to pod
              type: back-end 
```

## Namespaces 

Default - created auto by k8s when cluster is setup
kube-system - all k8s systems.. automatically created
kube-public - resources available all users are created

Namespace - isolation:
you can create a Dev / Prod env or namespaces

each namespace can have policies and resource limits 

default namespace:
DNS is built in for same namespace
othername spaces should be

"servicename.namespacename.svc.cluster.local" 

cluster.local = default domain of k8s cluster
svc = service 

`kubectl get pods --namespace=kube-system` to list other namespaces

usename space option to create in different namespace

namespace: placeinMETADATAsection in def file

```yml
apiVersion: v1
kind: Namespace
metadata:
       name: dev
```

`kubectl create namespace <name>` another way to create namespace 

switch to namespace perm

`kubectl config set-context $(kubectl config current-context) --namespace=namespacename`

`kubectl get pods --all-namespaces` 

current-context = used to manage multiple clusters 

__resource quota__

```
#quota.yml
apiVersion: v1
kind: ResourceQuota
metadata:
       name: quota
       namespace: dev
spec:
       hard:
              pods: "10"
              requests.cpu: "4"
              requests.memory: 5Gi
              limits.cpu "10"
              limits.memory: 10Gi
```

## Imperative vs Declarative 

imperative appraoch - giving step by steps instructions
declarative approach - giving what to do, but let system figure it out


IaC - Imperative approach is: set of instructions like provision VM, install sw, config config files, load webpages from git repo, start nginx server

IaC - Declare what we need.. using yaml files and everything needed to get infrastructure in place is done by software

Imperative k8 commands:
`kubectl expose deployment deployname --port 80` 
`kubectl set image deploy deployname image=imageversion:1`

Declarative k8 files:
create files that have desired state.
use `kubectl apply command` to bring infra to desired state

Imperative methods:
Create objects then Update objects 
create --- run, create , expose
Update --- edit, scale, set image

good for exams, etc... 

Imperative Object COnfig files:

kubectl create -f nginx.yaml

Update Objects:
kubectl edit deployment <object name>


this is ran in K8s memory. these changes will only last for that session. 

you can edit the deployment
then use `kubectl replace -f filename.yml` to make those changes stick in the file. you may need to use `--froce` to completely delete and replace objects 

with create -- if object exists error 
replace --- error if object doesn't exist

Imperative is very taxing.. cause need knowledge of current config

# Declarative appr

create objects:

`kubectl apply -f file.yaml` 
`kubectl apply -f /path/to/config-files`

update objects:

just run kubectl apply file

better approach because it allows k8s to decide on how to provision. it's the best way to create objects

`kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml`

`kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`

imperative commands 

`k pod redis --image=redis:alpine -l tier=db` to create labls
`k run custom-nginx --image=nginx --port=8080` to expose a port on creation


`k create svc clusterip httpd --tcp=80` 

`k run httpd --image=httpd:alpine --port=80 --expose`

`k create svc --help` 
to get help with the service

## Kubectl Apply cmd
converts yaml file to json file and then stores that in the state.
then compares changes to the state. and then makes those changes

annotations: 
       kubectl.kubernetes.io/last-applied-configuration:

is where the declarative objects are stored.. only stored there with the applyied command

local file, last applied config, and k8s


# Scheduling 

## Manual Scheduling

How scheduling works:
pod-definition file. Usually has a `nodeName:` 
schduler looks for pod that doesn't have a name
scheduler bind's name pod to node

No Scheduler means pod will be in a pending state.

to manually assign a pod is to set `nodeName` field.. name can only be assigned during creation of a pod. 

other way is to create a binding reuqest to posts api

```yaml
apiVersion: v1
kind: Binding
metadata:
       name: nginx
target:
       apiVersion: v1
       kind: Node
       name: <name of target node>
```
this will mimick what shecudle does
then send POST request to node with binding data

`curl --header "Content-Type:application/json" --request POST --data {content of yaml.. converted to json} http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/` 

## Labels and Selectors 

labels allow you to identify items class, kind, etc
selectors allows you to filter objects 

Type, application, functionality 

app: App1
function: front-end, back-end, db, 

`pod-definition.yaml`

metadata:
       name: appname
       labels:
              app: app1
              function: front-end

`kubectl get pods --selector app=app1` one use case

k8s uses labels and selectors in ways for replicasets/deployments

spec:
       replicas: 
       selector:
              mathLabels:
                     app: app1

template = for pods
metadata = for replicaset
spec = configure selector for labels definied on the pod so the replicaset will pull the right pods

### Annotations
used to record other details for info purpooses
annotations:
       buildversion: 1.34 

## Taints and Tolerations

pod to node relationship == tains and tolerations 

taints and tolerations -- used to set restrictions on what pod can be scheduled on a node

without taints/tolerations pods are distrubted evenly 

taint can prevent pods to be placed on a node
tolerations will allow pods to be on a tainted pod if it has the same taint label 

`kubectl taint nodes node-name key=value:taint-effect`

taint-effect:
NoSchedule 
PreferNoSchedule
NoExecute -- new pods will not be scheduled on node, existing pods on node will be evicted if can't tolerate the taint

`k taint node node1 app=blue:NoSchedule`

tolerations - Pods

pod def file
spec:
       tolerations:
              - key: app
                 operator: "Equal"
                 value: "blue"
                 effect: "NoSchedule"

needs double quotes

taint and tolerations -- does not tell pod to go to a certain node.. but tells pods to go to certain  nodes if tolerations are met. 


master node by default has taint to not accept pods...
app workloads best practice not to schedule pods on master node


`kubectl describe node kubemaster | grep Taint` to view taints

to untaint node
`kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-` 
## Node Selectors 


use node selectors in the spec section

```yaml
spec:
       nodeSelector:
           size: Large # these are labels assigned to the nodes
```

nodes must be labeled
`kubectl label nodes <node-name> <label-key>=<label-value>`

## Node Affinity

can't do `or` or `not`
node affinity allows advanced pod placement 

```
spec: # must be in the container spec 
       nodeAffinity:
              requiredDuringSchedulingIgnoredDuringExecution:
                     nodeSelectorTerms:
                     - matchExpressions:
                            - key: size
                               operator: In # or NotIn
                               values:
                               - Large
                               - Medium

      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists

```
different operators allow you to modify the affinity rules

different types of node affinity

preferredDuringSchedulingIgnoredDuringExecution

DuringScheduling || DuringExecution 

Type 1 || when pod is created || Ignored
Type2 || Preferred || Ignored 


during execution means pod is already created. what happend to pods when they're running  on the node.. 

command to check labels

`kubectl get node node01 --show-labels` 

## Taints and Tolerations Vs Node Affinity

`kubectl label --help`

you can combine taints and tolerations and node affinity rules
to dedicate nodes to certain pods 

## Resource Limits

if no resources are available there pod creation will be paused.. 

CPU / MEM  / Disk

Are resource requests
Cpu 0.5 
256 Mi 


```
apiVersion: v1
kind: Pod
metadata:
spec:
       containers:
       name: 
       image:
       ports:
              - containterPort:
       resources:
              requests: "1Gi"
              cpu: 1

```

1 count of CPU means 
0.1 = 100m = milli 
1m is the smallest size

1 CPU = 1 AWS vCPU, 1GCP Core, 1 Aure core, 1 hyperthread


Gi = Gibibyte = 

1 Ki = Kibibyte = 1024 bytes
1 


Limits is used to keep pod from using too much
use the same resources

Limits allows the cpu to be throttled. 

>Note on default resource requirements and limits
In the previous lecture, I said – “When a pod is created the containers are assigned a default CPU request of .5 and memory of 256Mi”. For the POD to pick up those defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.
```
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container

    ---

apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

> The status OOMKilled indicates that it is failing because the pod ran out of memory. Identify the memory limit set on the POD.

k get pod elephant -o yaml > elephant.yaml

`kubectl replace -f elephant.yaml --force`. This command will delete the existing one first and recreate a new one from the YAML file.

## DaemonSets

Daemon sets - runs one copy of a pod per node on cluster
replicas are automatically added or deleted from new or deleted nodes

ensures one copy of pod is present on all nodes on cluster

useful for monitoring solutions / log viewer

kube-proxy is a daemon set

```yaml

appVersion: apps/v1
kind: DaemonSet
metadata:
       name: monitoring-daemon
spec:
       selector: 
              matchLabels: 
                     app: monitoring-agent
       template:
              metadata:
                     labels:
                            app: monitoring-agent
              spec:
                     containers:
                            - name: monitoring-agent
                               image: monitoring-agent
```

`kubectl get daemonsets` to get daemonset info

how do daemon sets work?
from v1.12 - uses NodeAffinity and default scheduler to assign Daemonsets

## Static Pods

kubelet relies on api server on what pods to load which gets that info from kube scheduler 

`/etc/Kubernetes/manifests` place pod definitions in this directory and will create pods on the host.. and will ensure the pod stays alive

this will create static PODs 

kubelet - only works at POD level. 

kubelet.server 
--config=kubeconfig.yaml

```
#kubeconfig.yaml
staticPodPath: /etc/kubernetes/filepath
```
good idea to troubleshoot to look at this kubelet.service to view and configure the method to setup static pods, etc
config option or path for the static pod stuff

use `docker ps` to view static pods 

static pods will be listed as any other pod. a mirror object will be created in the kube-apiserver in a ro state. 

Use Case:
deploy control plane components as static pods
apiserver, controller-manger, etcd, etc

kubeadm tool sets up static pods

i looked in the systemd dir in etc.. you can also 

ps -aux | grep kubelet to get the kubeconfig

`kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml`



## Multiple Schedulers 

cluster can have multiple schedulers at the sametime

`--scheduler-name=default-scheduler`
you can use a custom scheduler name

`/usr/local/bin/kuber-scheduler`
`/etc/kubernetes/manifests/kuber-scheduler.yaml`

```
- command:
   - kube-scheduler 
```

change metadata-> name: to use a customer scheduler
then add command `--scheduler-name=scheduler-name`

choose a leader to lead scheduler activities 

`kubectl get events`
look for scheduled events 

to view logs 
`kubectl logs my-custom-scheduler --name-space=kube-system` to view logs

`kubectl get serviceaccount -n kube-system` 

`kubectl get clusterrolebinding`

`kubectl create -n kube-system configmap my-scheduler-config --from-file=/root/my-scheduler-config.yaml` 


## Configuring K8s Scheduler 
Advanced reading 
https://github.com/kubernetes/community/blob/master/contributors/devel/sig-scheduling/scheduling_code_hierarchy_overview.md

https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/

https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/

https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work


# Logging & Monitoring 

## Monitor Cluster Components

Node level metrics 
cpu usage, mem, disk

pod level metrics
cpu memory, storage usage

k8s does not come with monitoring solution built in..

Metrics Server, Prometheus, elastic stack, datadog, dynatrace

Heapster --- deprecated and replaced by metrics server

1 metrics server per k8s cluster

Metric Server - in memory metrics solution 
unable to view historical data

cAdvistor - retrieving performance metrics through kubelet api to go to metrics server


`minikube addons enable metrics-server` 

others will nee dto be done using github

kubectl create -f deploy/1.8+ 

`kubectl top node or node` 

## Managing Application Logs

`kubectl logs -f pod-name`  needs container name if there is multiple inside pod


# Application Lifecycle Management 

## Rolling Updates and Rollbacks 

Rollout and Versioning in a deployment -- deployment createment creates a rollout or revision:

Rev 1: original
Rev 2: new rollout triggered and new version created

`kubectl rollout status <deployment/demployment name`
`k rollout history deployment/deploymentName` shows history

### 2 type of deployment strategies 

Recreate strategy -- not default
destroy ingall instances and then deploy the new version of app

Rolling Update -- update apps one by one
this is the default strategy.. 

_How do update deployments_

use `k apply -f deploymentFile.yml` 

or `kubectl set image deployment/name \ nginx=nginx=1.9.1` this will not update the deployment file though. so be careful 

 you can watch the replicasets getting updated with

 `kubectl get replicasets` to see replicasets get provisioned 

 *Rollback* the update

 `kubectl rollout undo deployment/deployment-name` deployment will destroy pods in new replicaset and bring back the old replicasets

## Application Configurations

### Configuring command / arguments on apps 


*commands / arguments in docker* 
containers are meant to run tasks or process. like webserver db, or computation
once task is complete it should exit. containers only run as long as process is alive

`ENTRYPOINT` allows a cli paramater in docker 
use `CMD` at end of file for a default paramater 

### Commands and args in k8s pod

create pod def template

in spec --> containers --> use `args:  ["10"]` inside of containers 
to overright entrypoint use `command: ["command name"]` which will overview entry point 


## ENV Variables in K8s

in spec -> containers
```
env:
       - name: NAME_OF_VAR
          value: var_value 

# other examples 
inside env block
valueFrom:
       configMapKeyRef: 

valueFrom:
       secretKeyRef: 
```


## Configure configMap in Apps

used to config data in key/val pairs to inject config map into the pod

1. create config map
2. inject into the pod

`kubectl create configmap` imperative way
`kubectl create -f configmapfile.yml` declarative way

`k create configmap <config-name> --from-literal=<key>=<value>`

`app-config --from-literal=APP_COLOR=blue`

`<config-name> --from-file=<path-to-file>`

```
apiVersion: v1
kind: ConfigMap
metadata:
       name: app-config

data:
       APP_COLOR: blue
       APP_MODE: prod
```

`kubectl get configmaps` 

envFrom:
       - configMapRef:
            name: app-config


how to inject from config map ^^^^

```

envFrom:
       - configMapRef:
              name: app-config

# single env
env:
       - name: APP_COLOR
          valueFrom:
              configMapKeyRef:
                     name: app-config
                     key: APP_COLOR

# volumes

volume:
       - name: 
          configMap:
              name: app-config
```



## Secrets


stored in hash format 

1. create secret
2. inject into pod


imperative way to create secret

`kubectl create sercet generic`

`app-secret --from-literal=<KEY>=<value>`

you can use multiple `--from-literals`

or use `--from-file <path-to-file>`


declarative approach

```
#secret-data.yaml

apiVersion: v1
kind: Secret
metadata:
       name: name-of-secret
data:
       DB_HOST: mysql
       DB_USER: root
       DB_PASSWD: password
```

data can't be in plaintext

`echo -n 'mysql' | base64` to convert to hash

`kubectl get secrets` to get secrets and you can use `desribe` to get secret values 

`kubectl get secret secret-name -o yaml` will reveal the hashed values of the secret.


to decode use the same pipe with `base64 --decode`

```
# inject all values as env
envFrom:
       - secretRef:
              name: app-secret

# inject single env
env:
       (list) name: DB_PASSWORD
       valueFrom:
              secretKeyREf:
                     name: app-secret
                     key: DB_PASSWORD

# volume method to inject secret 
volumes:
       (list) name: app-secret-volume
       secret:
              secretName: app-secret
```
each attribute is created as file in the boject

`ls /opt/secret-volume-name` 
will display files for each key:val pair


## Multi Container PODs

microservices and independent svc creation

how to jump in container
`kubectl -n elastic-stack exec -it app -- cat /log/app.log` 
use exec command 


3 common patterns:
sidecar which is similar to the log example in the CKA course

adapter and ambassador are the other two methods

## Init Containers 

> In a multi-container pod, each container is expected to run a process that stays alive as long as the POD’s lifecycle. For example in the multi-container pod that we talked about earlier that has a web application and logging agent, both the containers are expected to stay alive at all times. The process running in the log agent container is expected to stay alive as long as the web application is running. If any of them fails, the POD restarts.

 

>But at times you may want to run a process that runs to completion in a container. For example a process that pulls a code or binary from a repository that will be used by the main web application. That is a task that will be run only one time when the pod is first created. Or a process that waits for an external service or database to be up before the actual application starts. That’s where initContainers comes in.

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ;']
```

    
# Cluster Maintenance

# OS Upgrades 

pod eviction timeout.. time a pod remains down before it's dead to k8s

`kube-controller-manager --pod-eviction-timeout=5m0s` 

before doing an upgrade you can drain nodes of pods before doing upgrade

`kubectl drain node-1` and then node is cordon / marked as unscheduleable
`kubectl cordon node-2` does not drain nodes, but pauses pods from being scheduled on the node. 
`kubectl uncordon node-` will allow pods to be scheduled on node again

may need to use `k drain node01 --ignore-daemonsets` flag


## Kubernetes Software Versions



etcd cluster - own versions
CoreDNS - own version

kube api server, controller-manager kube-scheduler kubelet kubeproxy kubectl should have matching versions 

## Cluster Upgrade process

Core control plane components
can be at different release versions. 

kube-apiserver --- components shouldn't be higher than api server
controller-manager, kube-scheduler X-1
kubelet, kube-proxy X-2

kubectl X+1 > X-1 

these versioinings allow for live upgrading. 

K8s supports up to 3 recent minor version.. 

1.10, 1.11, 1.12 are supported. if 1.13 is released 1.10 isn't supported anymore


upgrade one minor version at a time. 

`kubeadm upgrade plan` or `kubeadm upgrade apply` 

upgrade master nodes first and then upgrade worker nodes


### kubeadm - upgrade

`kudeadm upgrade plan` gives info on what can be done.. 
current and available versioning 

kubelet's versions will need to be upgraded manually

`kubeadm upgrade apply v1.13.4`

must upgrade kubelet manually first

kubeadm=1.**

`kubelet=` upgrade kubelet via package manager or git

restart `systemctl restart kubelet`


## Backup and Restore 

what should be backed up in k8 cluster

- Resource Config - 
       - declarative file scan be stored in a repo like GitHub
       - `kubectl get all -all-namespaces -o yaml > all-deploy-services.yaml` will grab some resources groups
       - ARK or use Velero to backup k8s

- ETCD Cluster - 
       - hosted on master nodes. can config backup tool to back up etcd dat a dir
       - ETCDCTL_API=3 etcdctl save snapshot.db 
       - `snapshot status <file of db>`
       - stop kube-apiserver stop
       - `snapshot restore db file. --data-dir /var/lib/etcd-from-backup` will restore as new members of cluster
       - `systemctl daemon-reload` and restart etcd restart

- Persistent Volumes - can also be backed up

_etcd_ notes :
etcd is a static pod on the master nodes. lab is using version D. to specifiy api version export the variable 
export ETCDCTL_API=3

`etcdctl version `
`etcdctl snapshot save -h` keep a note of the mandatory global options 

example backup: 
`etcdctl snapshot save /opt/snapshot-pre-boot.db --endpoints 127.0.0.1:2379 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key` 

# Security 


## K8 Security Primitives

Secure hosts - root account access disabled, pw auth disabled, only ssh auth turned on

kube-apiserver - 
who can access cluster?
what can they do?

Auth:
Files: username and pw, username/tokens
certs
external auth providers - ldap
service accounts

What can they do - AuthZ

- rbac authz
- abac authz 
- node authz
- webhook mode

TLS Certificates
etcd cluster, kubelet, kube proxy, kube schedulur, kube controller manager


by default all pods can talk to each other. access can be restricted using network policies 

## Authentication 

k8s does not manage user accounts natively 
k8s can manage service accounts

`kubectl create serviceaccount sa1`

user access is managed by `kube-apiserver` `https://kube-server:6443`

kube-apiserver auths before processesing request

ways to authN
static pw file, static token file, certificates, identity services 

static pw / token files:
`user-details.csv` 
kube-apiserver.service 
--basic-auth-file=/path/to/file 

modify the kubeadm file of the kube-apiserver.yaml and add it

static pw file,
pw / or token, username,userid,group 


## TLS Basics

symmetric encryption - same key used for encryption and decryption
asymmetric encryption - pub and private key. or key pair

Private key - Public key (lock)... you can only lock something with the public key and the private key can decrypt it


`openssl genrsa -out private.key 1024` - private key
`openssl rsa -in public.key -pubout > publickey.pem` public key


https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04 

## TLS in K8s

server certificates - Certificate (public key) private key

CA - (root certificates) public and private key

Client certs - 

NAMING CONVENTIONS

| Certifcate (public key) | Private Key |
|---| ---|
| *.crt *.pem | *key *-key.pem |
| server.crt | server.key |
|server.pem | server-key.pem |


private key usually includes key in them or . pem

server certs of servers
client certs for clients


### kube-apiserver 
apiserver.crt (pub) apiserver.key 

### etcdserver
etcdserver.crt etcdserver.key

### kubelet
kubelet.crt kubelet.key 

client certs for clients

admin ---> needs keys to interact with kube-apiserver
kube-schedulur - has private and pub keys to access kube-apiserver
kube-controller-manager ---> needs crt and key to access 
kube-proxy ---> needs crt and key 

kube-api-server ---> can use same server keys to talk to etcd server or generate client servers 
same goes for kubelet server 

k8s requires at least one CA for cluster. multiple clusters can be used 

you can have a special CA for etcd server and have all client certs signed by it

## TLS in k8s --- Certificate Creation 

Ways to create Certs:

- EASY RSA
- OPENSSL
- CFSSL

server key process
generate keys `openssl genrsa -out ca.key 2048`
certifcate rsigning request `openssl req -new -key ca.key -subj "/CN=Kubernetes-ca" -out ca.csr`
sign certifactes - `openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt`

groups: system:masters should be inserted in the OU cert request `/OU=system:masters` 


`curl url \ --key admin.key --cert admin.crt --cacert ca.crt` in curl request 
or create kube-config file with all the cert info! **light bulb***

each service needs copy of root cert 

etcd server certs and then etcd peer certs

you can configure the peer-cert files and the key-file / cert-file certs

kube-apiserver- -- is kubernetes, kubernetes.default kubernetes.default.svc.cluster.local 
some may just use ip address 
all names should be present in kube-apiserver cert

`openssl genrsa -out apiserver.key 2048`

`openssl req -new -key apiserver.key -subj \ "/CN=kube-apiserver" -out apiserver.csr`

openssl.cnf file will need to be created to use alt_names 
then include the file in the command

```ini
[alt_nams]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster.local
IP.1 = 10.96.0.1
IP.2 = 172.17.0.87
```
`client-ca-file` every component needs it
`tls cert file, private and pub` `etcd cafile,cert file` etc

kubelet is a https server running on each client. 
needs a key / cert pair

certs are named after the node 

## View Certificate Details 

_The Hard Way_
deploy all certs by yourself if you do things the manual way
`cat /etc/systemd/system/kube-apiserver.service`
deploys on nodes

_kubeadm_
automatically generates certs
`cat /etc/kubernetes/manifests/kube-apiserver.yaml`
deployed as PODs


### kubeadm

checkout the command to start kube-apiserver
note the path

then view the file path
`openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout`
look at `Subject` `Subject Alternative Name` and view `Issuer:`


if container is not running and it's a kube-system check the docker container nd logs 
2379 is etcd port for k8s

## Certificates API

CA = just a pair of key crt files that were generated. whoever gets access to CA files can create as many users as possible. 
files should be protected. which will become CA server


k8s has built in API to generate certs and signing requests

1. Create CertificateSigningRequest Object
2. Review Requets
3. Approve Requests.
4. Share requests


`openssl genrsa -out jane.key 2048` creates key
`openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr

create manifest file for the user
apiVersion: certifcates.k8s.io/v1beta1
kind: CertifatesSigingingRequest
metadata:
       name: <users name>
spec:
       groups:
       - system:authenticated
       usages:
       - digital signature
       - key encipeherment
       - server auth
       requests

cat jane.csr | base64 | tr -d "\n" or do `base64 -w 0` to get redit of new lines 

`kubectl get csr`
`kubectl certificate approve jane`


controller manager - CSR APPROVING - CSR-SIGNING for carrying out cert processes
base64 is needed for decryption or decoding 


## Kubeconfig

if you want you can send curl request to api to grab info on pods

`curl https://IPOFK8s:6443/api/v1/pods  --key admin.key --cert admin.crt --cacert ca.crt` 

`kubectl get pods --server URL --client-key admin.key --client-certifacete admin.crt --certifacte-autority ca.crt` are the flags

`$HOME/.kube/config` is the default location of kubeconfig 


config file is divded into 3 sections:
- clusters
       - each k8s cluster
       - server field 
- Contexts
       - Admin@production (marry the user account to access which cluster)
- Users
       - user accounts ie: admin, dev user, prod user
       - client-keys go here

example is in k8s manifest in vs code


add `current-context: my-kube-admin@my-k8s-server` to set a default context to use

`kubectl config view` to view current context and if not specified you it will use default

`kubectl config view --kubeconfig=path-to-config` to show path

to update current context

`kubectl config use-context theUser@theK8sPlace` 
`kubectl config -h` to change stuff 

you can use `certificate-authority-data` to paste in the certs if you like 
must be base64 format


to swtich context you need to use the name of the context


## API Groups
`/version` 

k8s api is grouped sections based on purpose

metrics, healthz, api, apis, logs are all different paths

`/api` core group
`/apis` named group

core is all core functionality 
`/api/v1/` namespaces,pods,deployments, configmaps etc


new features are in `/apis` apps, extensions, networking, storage, authentication, certifates
inside aps
deployments, replicasets
statefulsets


in networking networking policies

actions for API are called verbs

you can use the docs to see the different group and verbs

`curl https://localhost:6443 -k` 

```bash
curl https://localhost:6443/version --key client-admin.key --cert client-admin.crt --cacert server-ca.crt
{
  "major": "1",
  "minor": "24",
  "gitVersion": "v1.24.3+k3s1",
  "gitCommit": "990ba0e88c90f8ed8b50e0ccd375937b841b176e",
  "gitTreeState": "clean",
  "buildDate": "2022-07-19T01:08:03Z",
  "goVersion": "go1.18.1",
  "compiler": "gc",
  "platform": "linux/arm64"
}
```

kubectl proxy allows you to access the cluster locally so you can access https stuff

kube proxy != kubectl prxy 

all resources in k8s are grouped into different api groups. core groups you have named groups -> then api groups -> resources -> actions called verbs

## Authorization in K8s

auth methods: 

_Node_
user accesses Kube API --> Kubelet read services,endpoints,nodes pods and writes status
kubelet uses Node Authorizer handles the requests 

_ABAC_ -- access based access control 
external access to API
can view PODs, Can create PODs, can delete Pods

to control access you create a policy file
`{"kind": "policy", "spec": {"user": "dev-user", "namespace": "*", "resource":  "pods", "apiGroup": "*"}}`

this policy must be edited manually after changes and then restart the API server. tough to manage


_RBAC_
create a role that's associated with the permissions. then assign or associate the users to that role

just need to modify role and it will take effect immediately. desired way to control auth

_Webhook_
to outsource auth mechanisms 

Open Policy Agent -> k8s would make the call to the agent and then it will decide

`AlwaysAllow` or `AlwaysDeny` does what the name states.. 
AlwaysDeny could be configured

mode is set in `--authorization-mode=` and it's set to AlwaysAllow by default

denied checks go through the chains to the next object. unless it goes to the end.. once request is approved access is granted

## RBAC

How to create a role

```
#manifest needed 
apiVersion: rbac.authorization.k8s.io/v1
kind: Role 
metadata:
  name: developer
rules:
  - apiGroups:
      - 
    resources: ["pods"]
    verbs: ["list", "get", "create", "update", "delete"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding 
metadata: 
  name: devuser-developer-binding 
subjects:
  - kind: User
    name: dev-user
    apiGroup: rbac.authorization.k8s.io 
  roleRef: 
    kind: Role 
    name: developer 
    apiGroup: rbac.authorization.k8s.io 
```

binds user to the role 
use kubectl create 
this falls under the namespaces 
namespace should be specified in the metadata

`kubectl get roles` or `kubectl get rolebindings`  you can use the flag `--no-headers` to omit headers

how to check access 
`kubectl auth can-i "action you'd like to dlo"`  `--as UserName` flag to check users perms 

## Cluster Roles


resources are categorized as namespaced or cluster scoped

cluster scoped resources
nodes, persisten volumes, clusterroles, clusterrolebindings, certifactesigning requests, namespace objects


`kubectl api-resources --namespaced=true` set false to view cluster scoped resources 

_clusterroles_ are for cluster scoped resources... 
like cluster admin / storage admin.. 

```

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin 
rules:
  - apiGroups:
      - ""
    resources:
      - "Nodes"
    verbs:
      - list
      - get 
      - create 
      - delete 
```

Link user to cluster role

cluster role binding link user to  role
```
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
  - kind: User 
    name: cluster-admin 
    apiGroup: rbac.authorization.k8s.io 
roleRef: 
  kind: ClusterRole 
  name: cluster-administrator 
  apiGroup: rbac.authorization.k8s.io 
```

a cluster role can be created to namespaced resources. user will have access to resources across all namespaces 


## Service Accounts 

12% ---> Know how to config auth and authorization 
Understand k8s security prim

2 account types: User / SVC account

service acconts like Prometheus , Jenkins, etc usually a bot

`kubectl create serviceaccount <account name>`

`kubectl get serviceaccount` will list all service account

`kubectl describe serviceaccount <account name>` will generate token
it's stored as a secret... 

stoken is stored inside secret object. and linked to service account
`kubectl describe secret <token name>` token can be used as auth Bearer token... 


apps could be deployed on the cluster itself --> can be easily simple by mounting service account secret as a volume on the Pod or deployment

so it can be easily read by the app

there is a deafult serviceaccount that exists. for every namespace a default has it's own defautl svc acct

`kubectl exec -it <pod name>` 

inside spec

`serviceAccountName: account-name` you can't edit the svc of Pod without recreation

you can edit inside a deployment as it will do the rollout automatically 

`automountServiceAccountToken: false` to disable default automount svc account token

## Image Security 

`image: library/nginx` if no user or account name provided means library come from docker library image

google images are stored at `gcr.io`
`gci.io/kubernetes-e2e-test-iamges/dnsutils` 

hosting private internal registry 

`docker login privater-regsitryaddy.io`

how to login to private registry in k8s
```yaml
kubectl create secret docker-registry regcred \
       --docker-server=...
       --docker-username=
       --docker-password=
       --docker-email=

```

use `imagePullSecrets: name: [""]` 

## Pre-req - Security in Docker

### process isolation 
containers are not completely isolated from the host --- share the same kernel

containers are isolated using namespaces in linux.. host has namespace and containers have a namespace

all processes run on the container are run on the host in their own namespace... docker containers can only see the processes in it's namespace 


by default docker runs processes as root.. for commands to not run as root.. use `--user=1000` basically set an ID for user or use `USER 1000` to avoid running it as rot

have user defined in docker image in time of creation 

to prevent root inside container to go buck wild. docker has policies to limit what container "root" can do.. baby root if you will. 

docker uses linux capabilities to prevent that root to do stuff... 

view `/usr/include/linux/capability.h` to view all that root can do
to control and limit what capabilities are available to a user 

use `--cap-add <OPTION>` to change the capability of what root can run
vice verse `--capp-drop` `--priviledged` allows full control 

view `MAC_ADMIN`

## Container Security 

security settings can be done at container level or POD
if at POD level will carry over the POD
settings on container override containers on the pod (if you configure both)

inside spec
`securityContext: runAsUser: 1000` to set the user to 1000 inside the pod 
to set the same config move that section under the container specs

capabilites can be added to the container level and not the pod level
under `securityContext:` add a dictionary for `capabilites: add: ["MAC_ADMIN"]` 


## Network Policies 

incoming traffic <--- ingress
outgoing traffic ---> egress

### Network Security 

each node has ip addy
and each service has ip addy

whatever solution implemented pods should be able to speak without additional routes

all pods are in vpn between eachother... can reach eachother using ips, pod names, or svc name

k8s is configured with "all allow" for any pod or service within the cluster

implement network policy are linked to one or more pods. 

only allow ingress traffic from api pod on port 3306.. only the filtered traffic will be allowed to open a socket.

use lables and selectors in network polciy 

```yaml
policyTypes: 
- Ingress
ingress:
- from: 
  - podSelector:
         matchLables:
                the: label:
  ports: 
  - protocol: TCP
     port: 3306


apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
       name: db-policy

spec:
       policyTypes: 
- Ingress
ingress:
- from: 
  - podSelector:
         matchLables:
                the: label:
  ports: 
  - protocol: TCP
     port: 3306
``` 
solutions that support network policies:
- kube-router 
- calico
- romana
- weave-net

unupoorted
- flannel 

## Developing Network Policies 

this will block all traffic that doesn't match the selector
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
       name: db-policy
spec:
       podSelector:
              matchLabels:
                     role: db
       policyTypes:
       - Ingress 

       ingress:
       - from:
              - podSelector:
                            matchLablers:
                                   name: api-pod
        ports:
        - protocol: TCP
           port: 3306
```

you only need a rule for ingress.. egress is allowed for the type of response to the Ingress request 


when deciding on the type of rule needed. you should decide on the direction the traffic needs to go.. 

under pod selector add
`namespaceSelector: {matchLabels: {name: <namespace>}}

if node is not in the cluster. you can configure a policy allowing traffic from certain IP addressesblock

using an IP this will go in ingress: from section 

```
- ipBlock:
       cidr: 192.168.0.100/24
```
inside the from block it works as an or if they are in list format.. 


```yaml
egress:
- to: 
       any selectors work here 

   ports: 
       0 protocol: TCP
          port: 80
``` 

`kubectl get networkpolicy | netpol` 

# Storage 


## Intro to Docker Storage 

## storage drivers - 
File System
`/var/lib/docker` --- this is where docker store data/files related to images/containers related to docker host
aufs, containers, image, volumes

_Layered architecture_ docker builds images in a layered archeticture. each instruction is a layer. 
each layer only stores changes in the previous layer 

image layers are ro.. 

docker creates a new writable layer on top of the image layer. 
container layer (layer only lives while container is running).  once container is dead all files are dead and destroyed... rw 

docker uses `copy-on-write` when changes need to be made on image changes.. a copy is made of the file that's being changed. 

adding a persistent volume to keep changes

`docke volume create data_volume` creates a folder `/var/lib/docker/volumes/<volume_name>`

`docker run -v volume_name:/var/lib/mountPoint ImageName` use the path where you see /var/lib/MountPoint

docker will automatically create volume if you just use the -v volume command without the volume create

this is called volume mounting. because you're mounting in the docker var lib area

`docker run -v /path/on/host/tomount:/path/on/container imageName` this is a **bind** mount 

`--mount ` is the way to mount now in docker 

`docker run --mount type=bind,source=/path/tohost,target=/container location ImageName` 

docker uses storage drivers to do layered architecture: AUFS,ZFS,BTRFS, Device Mapper, Overlay, Overlay2
AUFS is default ubuntu storage driver

Device Mapper is good for cross platform.. docker chooses best storage driver avaialble based on operating system... 


## volume driver plugins - 

default volume driver plugin is local 

Azure FIle Storage, Convoy, Digital Ocean Block Storage, PortWorx, different volume drivers 
support different storage providers 

docker options: `--volume-driver volume/driver` you can provision a volume from the cloud. 

## Container Storage Interface 

standard how an orchestration solutoin like k8s would interface with other container runtimes 
CRI - container runtime interface
CNI  - container networking interface similar to CRI  but networking
CSI - container storage interface --- a universal standard to allow any storage vender to work with it

RPC - remote procedure calls --> createvolume, deletevolume, controllerpublish volume

CSI, storage driver 
should call to provision a new volume, should provision a new volume on the storage
should call to delete a volume, should decommission a volume
should call to place a workload that uses the volume onto a node, shoudl make the volume available on a node

## Volumes 

docker volumes are transient - only meant to be used for a short amount of time

```
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
  labels:
    name: random-number-generator
spec:
  containers:
  - name: random-number-generator
    image: alpine
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"

    command: ["/bin/sh", "-c"]
    args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
    volumeMounts:
      - mountPath: /opt 
        name: data-volume

  volumes:
    - name: data-volume
      hostPath:
        path: /data
        type: Directory 
```
only good for one node 

K8s supports, NFS,GlusterFS flokcer, aws, google persistent disk 


## Persistent Volumes 

allows central management of storage: creates a pool of storage 

Persistent Volumes (PVs) - clusterwide pool of storage volumes configed by sysadm

Persistent Volume Claims (PVCs) allow users to pool storage

accessModes: ReadONlyMany,ReadWriteOnce,ReadWriteMany

`kubectl get persistentvolume` 


## Persistent Volume Claims 

k8s binds PV to PVCs based on requests and properties set on the volume

every PVCs is bound to a single PV. during binding process looks for PV that has sufficient capacity, access modes, volume modes, storage class as the claim

if multiple matches of claim.. you can use labels / selectors to bind to right volume.
small claims can get bound to larger volume if they are no better options.. there is a 1:1 relationship between volume and claims 

PVC - pending if no PV are avaialble. 

`kubectl get perisstentvolumeclaim` 

`persistentVolumeRelcaimPolicy: retain` is default policy.. can be changed to Delete 
or can be set to Recycle to scrub data before amaking it available to other claims 

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
``` 

The same is true for ReplicaSets or Deployments. Add this to the pod template section of a Deployment on ReplicaSet.


## Storrage Classes 

Static Provisioning -- means creating things manually

dynamic provisioning -- automatically provision storage on different cloud platforms.. and automatically attach that to Pods


create stroage class version

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
       name: google-storage
provisioner: kubernetes.io/gce-pd 
parameters:
       type: pd-standard
       replication-type: none
```

in pvc-defintion use
`storageClassName: <storageclass name>`


# Networking 


## Network Namespaces


when container is created it's created with its own namespace
has a vnic and virtual 


`ip netns add <namespace>` 
`ip netns`

`ip netns exec <namespace_name> ip link` or `ip -n <namespace> link` run the link command inside the namespace

a virtual cable or PIPE to connect links

`ip link add verth-<namespace name> type veth pper name <namespace name peer>` 
then attach interface to each interface

`ip link set <veth-namespace> netns <namespace>`

Linux bridge or Open vSwitch to create a virtual switch

`ip link add v-net-0 type vridge` to create a bridge

`ip link set dev v-net-0 up` brings bridge up

connect name spaces to swtich

`ip link set <vnic > master <virtual bridge> `

`iptables -t nat -A POSTROUTING -s <subnet/24> -j MASQUERADE` 
`iptables -t nat -A PREROUTING --dport 80 --to-destintation <location of ip> -j DNAT`

## Docker Networking

`docker run --network none <image>` no network created in container

host network 
`docker run --network host <image>` will run on the host networking

bridge network - internal private network 
default `172.17.0.0/16` is the docker network 

container / network ns interchangeable 

`sudo iptables -nvL -t nat` to view iptables rules 

`docker network ls` list docker networks 

## Container Networking Interface (CNI)

use bridge

- create network namespace
- Create bridge network / interface
- create VETH Pairs (Pipe, Virtual cable)
- Attach vETH to Namespace
- Attach other vETH to Bridge
- Assign IP Add
- Bring the interface up
- Enable NAT - IP Masquerade

`bridge add <ContainerID> /var/run/netns/<namespaceid>`

bridge program is a plugin for CNI

docker uses CNM - container network model ... docker does not normally use cni plugins since it has it's own plugins

k8s invokes cni plugings manually


## Cluster Networking

each node needs 1 nic with unique hostname / mac addy

ports: 6443 api, kubelet 10250, kube-scheduler 1021, kubectrl manager, 1052

services 30000 - 32767 , 2379,2380 for etcd

### Important Note about CNI and CKA Exam

**An important tip about deploying Network Addons in a Kubernetes cluster.**

In the upcoming labs, we will work with Network Addons. This includes installing a network plugin in the cluster. While we have used weave-net as an example, please bear in mind that you can use any of the plugins which are described here:

[https://kubernetes.io/docs/concepts/cluster-administration/addons/](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

[https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model)

In the CKA exam, for a question that requires you to deploy a network addon, unless specifically directed, you may use any of the solutions described in the link above.

**_However,_** the documentation currently does not contain a direct reference to the exact command to be used to deploy a third party network addon.

The links above redirect to third party/ vendor sites or GitHub repositories which cannot be used in the exam. This has been intentionally done to keep the content in the Kubernetes documentation vendor-neutral.


## Pod Networking

k8s expect
every pod should have an IP add
every od should be able to comm with every other pod in the same node
every pod shoudl be able to com with every other node on other nodes without NAT

Container Network Interface (CNI) -
has a ADD section and a DEL section 

kubelet will look for --cni-conf-dir/etc/cni/net.d
--cni-bin-dir=/etc/cni/bin

then script is called


## CNI in k8s

configured in kubelet service

`ps -aux | grep kubelet`
`--network-plugin=cni` the configuration chose

multiple files and picks first file in alpha order

## CNI weave

weaveworks

Weave CNI plugin

deploys service on each node -- each peer stores topology of setup
weava creates a bridge on each network 

deploying weave -- as service or daemon as manually
can also be deployed as a pod

cni is stored in `/opt/cni/bin` -path

cni is stored in `/etc/cni`


`kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')&env.IPALLOC_RANGE=10.50.0.0/16"`


`kubectl apply -f "URL"` 

## IPAM (weave)

CNI - must take care of IP assignment.. should assist with avoiding duplicate IPs... there will be an IP-list database 


CNI - DHCP or host-local

ip= = get_free_ip_from_host_local()

`cat /etc/cni/net.d/net-script.conf` 

weave default network is 10.32.0.0/12
10.32.0.1 > 10.47.255.254 == 1,048,74... they split the ranges between nodes

## Service Networking

ClusterIP available through entire service internally 
NodePort - exposes service to external network with an open port in the 30000 range. App is also available internally 

the service is just a virtual object. -  assigned IP address from predefined ranged

creates forwarding rules on each node on teh cluster. 
IP of svc gets forwarded to <service ip and port> 

kube-proxy supports userspace - listening to the port
ipvs - 
iptables -  default 

`kube-proxy --proxy-mode` how to set it.

iptables set by kube-proxy


`kube-api-server --service-cluster-ip-range ipNet` to find default range 

iptables nat table output

`iptables -L -t nat | grep db-service` 

`cat /var/log/kube-proxy.log` to see which proxier is being used 





## DNS in K8s

whenever service is created k8s dns service creates a web record for it
you can then use service name to access service 

to access different namespaces.. <service-name>.<namespace>

all services are grouped into subdomain called svc

`<service>.<namespace>.<svc>` 

root domain is <cluster.local>` the fqdn 

recrods for pods not created by default. 
Pod records IPs are converted to dashed hostname. 192.168... is 192-168
type is called pod

## CoreDNS in K8s

after 1.12 dns is called CoreDNS

deployed as pod in kubesystem.. 2 pods for HA for replicaset inside of a deployment 

needs config dns. `/etc/coredns/Corefile` 

shows errors, health, kubernetes --- allows k8s to work with CoreDNS
pods option 
prometheus port
proxy
cache 
reload

```
kubernetes cluster.local in-addr.arpa ip6arpa
       pods insecure
       upstream
       fallthrough in-addr.arpa ip6.arpa
```
arpa (address and routing parameter area)  


CoreDNS is called `kube-dns` and the IP is the nameserver IP in pods 

kubelet has IP of DNS server. 

to get coredns config file look at the args section 

## Ingress

allows SSL security and single URL .. it's similar to a layer 7 lb. 

Ingress needs to be published as NodePort or as a load balancer

1. deploy - Nginx, HAPROXY, TRAEFIK
2. Configure routes 


Ingress Controller - (Step 1)
Ingress Resources - (Step 2)

k8s does not come with an ingress controller by default. 


### Ingress Controller
Needs to be deployed. 
Nginx, HAPRoxy, Traefik, GCP GFE LB 
Nginx / GCP is supported by k8s 


```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  selector:
    matchLabels:
      app: nginx-ingress-controller
  template:
    metadata:
      labels:
        app: nginx-ingress-controller
    spec:
      replicas: 1
      containers:
      - name: nginx-ingress-controller
        image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"

      args: 
        - /nginx-ingress-controller 

```

nginx has --- err-log-path keep-alive - ssl-protocols
must be passed as config map to controller

`kubectl get ingress` 
use rules to route based on different conditions 

Now, in k8s version 1.20+ we can create an Ingress resource from the imperative way like this:-

Format - `kubectl create ingress <ingress-name> --rule="host/path=service:port"`

Example - `kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"`

Find more information and examples in the below reference link:-

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-ingress-em- 

References:-

https://kubernetes.io/docs/concepts/services-networking/ingress

https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types


Steps to creating incress:

1.  Create Ingress name space
2.  Create configmap inside ingress ns
3.  create service account inside ingress ns
4.  create roles and role bindings for that space
5.  create ingress deployment
6. create service in that space


# Design and Install a K8s Cluster

Before design cluster

purpose: education, dev & testing, hosting prod apps --- education minikube --- single node clsuter
cloud or on prem 
Workloads: how many, waht kind? web, big data/analytics
Apps Resource Requirements

production nodes --- up to 50k nodes, up to 150k pods, up to 300k containers, 100 pods per nodes
HA multi node cluster with multi masters
kubeadm or GCP or Kops or AWS 


Nodes -- 1 master x 2 worker nodes.. I'm doing 2 masters x 2 Workers

Best practice is not host workloads on master nodes. 

Minikube deploys VMs -- single node cluster
KubeADM requires VMs to be ready -- singe / multi nodes

turnkey solutions --- provision it your self..  you provision vms, you config vms, you use scripts to deploy cluster you maintain vm, 

Hosted -- KaaS -- maintained by provider. 

OpenShift -- built on top of K8s... By RedHat.. Will look into i

Vagrant k8s solution 

hosted--- EKS , GKE , OpenShift, Azure k8s 


## Config HA 

multi masters incase you lose a master in the cluster so you can minimize the points of failure. 

Master --- ETCD, API Server, Controller Manager, Scheduler 

ApiServer -- can be active on multiple servers. as it handles a request at one time.. 6443... kube-config.. goodidea to have load balancer infront of k8s api server. use nginx or HA proxy 

Controller Manager / Scheduler --- Constantly looking at state. if they run in parallel can duplicate actions. only one should run at at time. should work in Active / Standby mode 
       CM - `kube-controller-manger --leader-elect true [other options]` which ever node respons first become leading lease of leader elect
              -- `leader-elect-lease-duration 15s` default 15's `leader-elect-renew-deadline 10s` --`leader-elect-retry-period 2s` will retry every 2 seconds

scheduler has the same approach as CM

ETCD - two topologies.. ETCD stacked topology --- Easy setup, easty to manage, fewer servers, risk during failures. 
seperate ETCD topology. Exernal ETCD. requires twice as many servers as node 

`cat --etcd-servers=` you can use multiple etcd... its a distributed system it's given a list because multiple servers can have it. 

## ETCD in HA

ETCD - distributed relaible key-val store that is simple, secure, & fast... a database


### Disributed 
datastore can be across multiple servers. maintaining identical copy of all data 

you can rw on any instance at the same time. 

READ --- all can read

WRITE --- only one instance is responsible for processing writes.
internally nodes elect leader

leader processes writes.. leader then pushes it to other instances 
write is verified once eac

leader election - RAFT 
uses random timers for initiating requests 
if downtime re-election takes place 

ETCD is HA. write is complete if it is written across the majority

once missing nodes come online data is copied

**Quorum** = N/2 + 1 ... minimum # of nodes needed for functionality properly 

Fault tolerance:  instances minus quorum gives fault tolerance. 

recommended to use odd number of nodes 

initial-cluster-peer option to setup the peers

etcdctl - version 2 is default
ETCDTL_API=3 to set up the 


## Troubleshooting k8s

check service status and endpoint
restarts gives idea if app is healthy

`kubectl logs -f or --previous` to follow or see previous pods logs 


## Worker node Failure

each node has set of conditions on why a node may have failed. 
status T/F/ unknown

disk pressure means disk compacity is true 
PID pressure when too many processes and memory low

when node crashes status is Unknown.. last heartbeat may show when node crashed



>The kube-proxy pods are not running. As a result the rules needed to allow connectivity to the services have not been created.

>Check the logs of the kube-proxy pods kubectl -n kube-system logs <name_of_the_kube_proxy_pod>

>The configuration file /var/lib/kube-proxy/configuration.conf is not valid. The configuration path does not match the data in the ConfigMap. kubectl -n kube-system describe configmap kube-proxy shows that the file name used is config.conf which is mounted in the kube-proxy damonset pods at the path /var/lib/kube-proxy/config.conf

>However in the DaemonSet for kube-proxy, the command used to start the kube-proxy pod makes use of the path /var/lib/kube-proxy/configuration.conf.

>Correct this path to /var/lib/kube-proxy/config.conf as per the ConfigMap and recreate the kube-proxy pods.

>Here is the snippet of the command to be run by the kube-proxy pods:


# JSON Path In k8s

kube-api uses json by default 

## JSON Path query 

- Identify kubectl command
- `kubectl get <object> -o json` 
- form json path query  .items or .object 
       - basically use ojbect notation 
- kubectl get pods -o=jsonpath= <query that was used.`
use '{ <json apth insert here> } '
you can use wildcard in items list

accepts `{"\n or \t"}` to create newline or tab

you can also use Loops - Ranges 

'{range .items[*]}  {.metadata.name} {"\t"} {.status.capaticity.cpu}{"\n}}'

kubectl get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>

`kubectl get nodes --sort-by= <JSON PATH QUERY>` to sort by name or cpu count 


```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secret-1401
  name: secret-1401
  namespace: admin1401
spec:
  containers:
  - command:
    - sleep
    - "4800"
    image: busybox
    name: secret-admin
    resources: {}
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/secret-volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always

  volumes:
  - name: secret-volume
    secret:
      secretName: dotfile-secret
status: {}
```
