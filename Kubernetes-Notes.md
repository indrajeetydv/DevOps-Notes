

@@ K8S Manifest YMLS : https://github.com/ashokitschool/kubernetes_manifest_yml_files

=================
Kubernetes (K8S)
=================

=> It is free and open source s/w

=> K8S developed by google company.

=> K8S provides Orchestration platform.

Note: Orchestration means Management.

=> Using k8s we can manage docker containers

	Ex: create, stop, start, scale up, scale down and delete.

=====================
Docker vs Kubernetes
=====================

Docker : containerization platform.

Note: Using docker we can package our "application code + application dependencies" as single unit for execution.

Note: Once docker image got created then k8s work will start.

Kubernetes : Orchestration platform.

=======================
Kubernetes Advantages
=======================

1) Free Of cost : No need purchase

2) Orchestration Platform : Manages our application containers

3) Self Healing : If any container got damaged then it will replace with new container. We can achieve High Availability.

4) Load Balancing : It will distribute incoming requests to all containers in round robbin fashion.

5) Scalability : Increase and decrease containers based on demand
   (Auto Scaling)

=========================
Kubernetes Architecture  
=========================

=> K8S will follow cluster architecture.

Cluster : Means Group of servers/machines.

=> In K8S cluster we can see below components

		1) Control Plane / Master Node

		2) Worker nodes

=============================
K8S Architecture Components
=============================

0) Kubectl : 

1) Control Plane

		- API server
		- Schedular
		- Controller Manager
		- ETCD

2) Worker Node

		- Kubelet
		- Kubeproxy
		- Docker
		- POD
		- Container


=> To deploy our applications in k8s cluster we have to communicate with Control Plane.

Note: To communicate with control plane we will use "kubectl" software.

=> API Server will recieve requests sent by kubectl and it will store requests in ETCD with pending status.

=> ETCD is an internal database used by k8s cluster.

=> Schedular will identify pending requests available in ETCD and it will schedule that task for execution in worker node.

Note: Schedular will use kubelet to identify worker node for task assignment.

=> Kubelet will act as worker node agent.

=> Kube Proxy will provide network required for cluster communication.

=> Docker engine is used to run docker containers in worker nodes.

=> In k8s everything will be represented as a POD only.

Note: docker container will be created inside the pod.

=> POD represents running instance of our application.

=> Controller Manager is responsible to check all the tasks are executing as expected or not in k8 cluster.

==========
K8S Setup
==========

1) Self Managed Cluster (we are responsible to setup everything)

		a) minikube (single node) (only for practice)

		b) kubeadm (multi node cluster)

2) Provider Managed Cluster (Ready made cluster for rent)

		a) AWS EKS

		b) Azure AKS

		c) GCP GKE


========================================
Minikube setup in windows for practice
========================================

1) Download and install docker desktop s/w

2) Verify docker installation by opending 'command prompt'

	Ex: docker run hello-world

3) Download and install minikube software	(.exe file)

4) Open Command prompt and start minikube cluster

Ex:

minikube start --driver=docker

minikube status

kubectl get nodes

kubectl get pods

kubectl create deployment nginx-deployment --image=nginx:latest --replicas=3

kubectl get pods

kubectl delete pod <pod-name>


==============
AWS EKS Setup
==============

EKS => EKS stands for Elastic Kubernetes Service.

=> EKS is a AWS cloud Provider Managed cluster.

Note: EKS is a paid service in AWS cloud.

=> EKS is the most trusted platform to deploy our applications.

@@ Steps To Setup : https://github.com/ashokitschool/DevOps-Documents/blob/main/05-EKS-Setup.md

Note: After practice don't forget to delete your EKS cluster.

===============
What is POD ?
===============

=> POD is a smallest building block in the k8s cluster.

=> Applications will be deployed as PODS in k8s.

Note: To create PODS we will use docker images.

=> To create PODS we will use Manifest YML file.

=> For single docker image we can create multiple PODS also.

=> If we run application with multiple pods then we will get High availability and Load Balancing.

=> PODS count can be increased and decreased based on the demand
  (scalability).




================
YML or YAML
================

=> YML/YAML stands for Yet another markup language.

=> It is used to store the data in human & machine readable format.

=> YML/YAML files will have extension as .yml or .yaml

=> Official Website :  https://yaml.org/

Note: indentation is very important in YML files.

============================
01 - Sample YML file data
============================

---
id: 101
name: Ashok
gender: Male
hobbies:
 - music
 - cricket
 - chess
 - dance
 - chatting
...


===========================
02 - Sample YML file data
===========================

---

person:
 id: 201
 name: John
 gender: Male
 address:
  city: Hyd
  state: TG
  country: India
 skills:
  - Java
  - Python
  - DevOps
  - AWS
...

=============================  

Write YML file to represent employee data with company and job details.

emp -> id, name, company and job

company -> name

job -> exp, type (permanent | contract)


---
emp:
 id: 101
 name: Ashok
 company:
  name: Microsoft
 job:
  exp: 11 Years
  type: permanent
...

#### Website To validate YML syntax : https://www.yamllint.com/

################### Use VS Code IDE to write YML Files ######################


======================
K8S manifest YML
=====================

=> In K8S manifest YML file we will write 4 sections mainley

apiVersion: <version-number>
kind: <resource-type>
metadata: <name>
spec: <container>

===================
POD MANIFEST YML
===================

---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
...

Note: Save above content in .yml file

# execute manifest yml
kubectl apply -f <manifest-yml-file>

# check pods
kubectl get pods

# check pod logs
kubectl logs <pod-name>

Note:  By default PODS can be accessible only with in the cluster. We can't access PODS outside the cluster directley.

=> To access PODS outside the cluster we need to expose them using K8S services concept.

===============
K8S Services
===============

=> K8S service is used to expose the pods for outside access.

=> We have 3 types of services in k8s

		a) Cluster IP
		b) NodePort
		c) LoadBalancer

====================
What is Cluster IP	
====================

=> POD is a short lived object.

=> When POD is damaged/crashed k8s will replace that with new POD.

=> For every POD new ip will be assigned.

Note: It is not at all recommended to use POD ip to access it.

=> Cluster IP service is used to group all the PODS and assign one static ip to access the pods.	

Note: Using Cluster IP we can access pods only with in the cluster.

Ex: Database pods we don't expose for outside access.

===========================
What is NodePort service ?
===========================

=> NodePort service is used to expose the pods to access outside of the cluster.

=> Using NodePort we can access the pods which are running in particular worker node.

Note: Burden will be increased on particular worker node.


===============================
What is Load Balancer service
===============================

=> Load Balancer service is used to distribute the load at worker node level.


=========================
K8S Service Manifest YML
=========================

---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080
    nodePort: 30070
...

# check k8s services 
kubectl get svc

# create service using above yml
kubectl apply -f <svc-yml>

# check services
kubectl get svc

# Open minikube tunnel for our application access
minikube service javawebappsvc

Note: If we deploy above application in EKS cluster then we should use node public ip to access our application.

		URL : http://worker-public-ip:node-port/java-web-app/

===================
What is NodePort ?
==================

=> When we use service type as NodePort then k8s will use one random port number to expose our application on worker node.

Node Port Range : 30,000 - 32767

Note: If we can also fix nodeport number in service manifest yml.


========================================
POD and Service in Single Manifest YML
========================================

---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080 
    nodePort: 30070
...

===============
K8S namespaces
===============

=> Namespaces are used to group the resources logically

		database-pods =====> database-ns

		backend-pods ====> backend-ns

		frontend-pods ===> frontend-ns


=> Inside k8s cluster we can create multiple namespace

=> Each namespace is isolated with another namespace.

# display k8s namespaces
kubectl get ns

# get pods of specific namespace
kubectl get pods -n kube-system

Note: In kubectl command if we don't specify any namespace then it consider "default" namespace.

=> In K8s we can create namespace in 2 ways

		1) using "kubectl create ns" command

		2) Using manifest YML file 

---------------
Approach-1 : 
---------------

# create namespace
kubectl create ns ashokitns

# delete namespace
kubectl delete ns ashokitns

Note: When we delete a namespace all the resources belongs to that namespace also gets deleted.

-------------
Approach-2 : 
-------------

---
apiVersion: v1
kind: Namespace
metadata:
 name: ashokit-backend-ns
...

# execute manifest yml
kubeclt apply -f <yml>

# get all resources belongs to ashokit namespace
kubectl get all -n ashokit


==========================
Namespace + POD + Service
==========================
---
apiVersion: v1
kind: Namespace
metadata:
 name: ashokit
---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 namespace: ashokit
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
 namespace: ashokit
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080
    nodePort: 30070
...

# run above yml file
kubectl apply -f <yml>

# check pods
kubectl get pods -n ashokit

# check service
kubectl get svc -n ashokit

# check all resources
kubectl get all -n ashokit

# open minikube tunnel to access app
minikube service javawebappsvc -n ashokit


========================================


=> When we create POD directley using "kind: Pod" in manifest yml then k8s will not manage pod life cycle.

=> If we delete pod then k8s will not create new POD.

=> If we want k8s to manage POD life cycle then we should use k8s resources to create the PODS.


1) ReplicationController (RC)
2) ReplicaSet (RS)
3) Deployment
4) DaemonSet
5) StatefulSet

==============================
What is ReplicationController
==============================

=> It is one of the resource in k8s to manage pod life cycle.

Note: If any pod is deleted/crashed/damaged then RC will perform self healing.

=> Using RC we can perform PODS count scale up and scale down

---
apiVersion: v1
kind: ReplicationController
metadata:
 name: javawebrc
spec:
 replicas: 2
 selector:
  app: javawebapp
 template:
  metadata:
   name: javawebapppod
   labels: 
    app: javawebapp
  spec:
   containers:
    - name: javawebappcontainer
      image: ashokit/javawebapp
      ports:
      - containerPort: 8080
...

kubectl apply -f rc.yml

kubectl get all

kubectl get pods

kubectl delete pod <pod-name>

kubectl get pods

kubectl scale rc javawebrc --replicas=5

kubectl scale rc javawebrc --replicas=1

============
ReplicaSet
============

=> It is one of the resource in k8s to manage pod life cycle.

Note: If any pod is deleted/crashed/damaged then RS will perform self healing.

=> Using RS we can perform PODS count scale up and scale down.

---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
 name: javawebrs
spec:
 replicas: 2
 selector:
  matchLabels: 
  	app: javawebapp
 template:
  metadata:
   name: javawebapppod
   labels: 
    app: javawebapp
  spec:
   containers:
    - name: javawebappcontainer
      image: ashokit/javawebapp
      ports:
      - containerPort: 8080
...


kubectl apply -f rc.yml

kubectl get all

kubectl get pods

kubectl delete pod <pod-name>

kubectl get pods

kubectl scale rs javawebrs --replicas=5

kubectl scale rs javawebrs --replicas=1

============
Deployment
============

=> It is one of the resource in k8s to manage pod life cycle.

=> This is the most recommended approach to deploy our applications in k8s cluster.

=> With Deployment approach we have below advantages

		1) Zero downtime
		2) Auto Scaling
		3) Rolling Update and Rollback

=> In Deployment we have 2 strategies to create PODS

		1) RollingUpdate
		2) Recreate

=> ReCreate means it will delete all existing pods and will create new pods.

Note: In This ReCreate approach, application will have downtime.

=> RollingUpdate means it will delete old pod and create new pod one after other.

---
apiVersion: apps/v1
kind: Deployment
metadata:
 name: javawebdeploy
spec:
 replicas: 2
 strategy:
  type: RollingUpdate
 selector:
  matchLabels: 
  	app: javawebapp
 template:
  metadata:
   name: javawebapppod
   labels: 
    app: javawebapp
  spec:
   containers:
    - name: javawebappcontainer
      image: ashokit/javawebapp
      ports:
      - containerPort: 8080
...

=====================================================================

Assignment : Setup K8S HPA (Horizontal POD Auto Scaler)

Reference Video : https://www.youtube.com/watch?v=c-tsJrcB50I&t=667s

======================================================================


=====================
ConfigMap & Secrets
=====================

=> For every application multiple environments will be available.

		1) DEV
		2) SIT
		3) UAT
		4) PILOT
		5) PROD

=> Every env will have its own config properties to run the application

Ex : 

1) database props

2) smtp props

3) kafka server properties

4) redis server properties

5) payment gateways

6) third party urls



=> If we configure above properties with in the application then our application will become tightly coupled.

=> When we want to deploy our application in another environment then we have to "change properties + re-package + re-create docker image + re-deployment". It is time taking process and risky.


=> If we want to deploy our app in multiple environments then we need to make sure our application is loosely coupled with env properties.

## To make our app loosely coupled with env properties we can use ## ConfigMap & Secrets" ##


=> Using configmap and secret we can de-couple application code and application properties so that our docker images will become loosely coupled.

Note: At the time of deployment, we can supply environment properties to the application container using ConfigMap & Secrets.

=> ConfigMap & Secret will store data in key-value format

=> ConfigMap is used to store non-sensitive data.
=> Secret is used to store sensitive data.


=========================
ConfigMap manifest YML
=========================

---
apiVersion: v1
kind: ConfigMap
metadata:
 name: configmap-dev
data:
  db_url: "jdbc:mysql://localhost:3306/"
  db_name: "ashokit"
  db_port: "3306"
---

$ kubect get configmap
$ kubectl apply -f <yml>

=========================
Secret manifest YML
=========================

---
apiVersion: v1
kind: Secret
metadata:
 name: secrets-dev
type: Opaque
data:
 db_username: YXNob2tpdA== #root
 db_password: YWJjQDEyMw== #abc@123
---

$ kubectl get secret

$ kubectl apply -f <yml>

===========
Assignment
===========

1) Deploy MySQL database in k8s cluster by using configmap and secret.

	DB name & DB user: read from config map
	DB root pwd  & user pwd  : read from secret


1) create configmap.yml (ex: mysql-configmap.yml)

2) create secret.yml (ex: mysql-secret.yml)

3) create deployment.yml (ex: mysql-deployment.yml)


---
apiVersion: v1
kind: ConfigMap
metadata:
 name: mysql-config-map
data:
 MYSQL_DATABASE: mydatabase 
 MYSQL_USER: myuser
...

---
apiVersion: v1
kind: Secret
metadata:
 name: mysql-secret
type: Opaque
data:
 MYSQL_ROOT_PASSWORD: cm9vdA==
 MYSQL_PASSWORD: cm9vdA==
....


---
apiVersion: apps/v1
kind: Deployment
metadata:
 name: mysql-deployment
spec:
 replicas: 1
 strategy:
  type: ReCreate
 selector:
  matchLabels:
   app: mysql
 template:
  metadata:
   labels:
    app: mysql
  spec:
   containers:
    - name: mysql-container
      image: mysql:latest
      ports:
       - containerPort: 3306

      env:
       - name: MYSQL_DATABASE
         valueFrom:
          configMapKeyRef:
           name: mysql-config-map
           key: MYSQL_DATABASE

       - name: MYSQL_USER
         valueFrom:
          configMapKeyRef:
           name: mysql-config-map
           key: MYSQL_USER

       - name: MYSQL_ROOT_PASSWORD
         valueFrom:
          secretKeyRef:
           name: mysql-secret
           key: MYSQL_ROOT_PASSWORD

       - name: MYSQL_PASSWORD
         valueFrom:
          secretKeyRef:
           name: mysql-secret
           key: MYSQL_PASSWORD
...


=================================
Blue - Green Deployment Approach
=================================

=> Blue green deployment is an application release model

=> It reduces risk and minimizes downtime

=> It uses two production environments, known as Blue and Green

=> The old version can be called the blue environment 

=> The new version can be known as the green environment

============
Advantages
============
=> Rapid releasing
=> Simple rollbacks
=> Built-in disaster recovery
=> Seamless customer experience
=> Zero Downtime


## Step-1 : Create blue deployment (pods will be created)

## Step-2 : Check pods status

## Step-3 : Create Live service to expose greebn pods pods (Type : Load 
Balancer)

		Ex : Selector is v1

## Step-4 : Access Load Balancer URL 

		Ex: http://lbr-public-dns/java-web-app/

## Step-5 : Create Green Deployment (pods will be created with latest docker image and label as v2).

## Step-6 : Verify green pods status

## Step-7 : Make green pods as live by changing "live-service" selector as 'v2' in yml file. After changing yml then re-execute it.

## Step-8 : Access load balancer URL and see the response of application.

		Ex: http://lbr-public-dns/java-web-app/

Note: If required we can switch from green pods to blue pods by changing live-service yml selector from v2 to v1.	

=====================
What is DaemonSet ?
=====================

=> It is one of the resource in k8s which is used to manage PODS life cycle.

=> It is used to create copy of the pod in every worker node.

Ex: FluentD pods will be created using DaemonSet.

======================
What is StatefulSet ?
======================

=> It is one of the resource in k8s cluster which is used create stateful pods.

Ex : ElasticSearch pods will be created using StatefulSet.


===========================
Application Log Monitoring
===========================

=> In Real-time application will be deployed with multiple pods for high availability.

=> Application pods will run in multiple worker nodes.

Note: As multiple pods running in multiple worker nodes for application multiple log files will be created.

Note: If any problem occurs in the application then to find root cause of the problem we need to check all log files of the application which is difficult task.

=> To overcome this problem we will use Log centralization concept.


=> To centralize log monitoring of the application we will use ELK/EFK stack setup.


ELK : Elastic Search + Log Stash + Kibana

EFK : Elastic Search + FluentD + Kibana

=====================================================================

Assignment : EFK Stack setup in K8S Cluster

@@ Reference video : https://youtu.be/8MLcbbfEL1U?si=bQ_BrOv3EiLu48eu

================
What is HELM ?
================

=> In linux we will use package managers to install a software

Ex : yum , apt, rpm etc...


=> HELM is a package manager which is used to install required softwares in k8s cluster.

=> HELM will use charts to install required packages.

=> Chart means collection of configuration files (manifest ymls).



=> Using HELM chart we can install promethues server

=> Using HELM chart we can install grafana server


==================
Helm Installation
==================

$ curl -fsSl -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3

$ chmod 700 get_helm.sh

$ ./get_helm.sh

$ helm


-> check do we have metrics server on the cluster

$ kubectl top pods
$ kubectl top nodes

# check helm repos 
$ helm repo ls

# Add the metrics-server repo to helm
$ helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/

# Install the chart
$ helm upgrade --install metrics-server metrics-server/metrics-server

$ kubectl top pods

$ kubectl top nodes


=========================
Kubernetes Monitoring
=========================

=> We can monitor our k8s cluster and cluster components using below softwares

1) Prometheus

2) Grafana

=============
Prometheus
=============

-> Prometheus is an open-source systems monitoring and alerting toolkit.

-> Prometheus collects and stores its metrics as time series data

-> It provides out-of-the-box monitoring capabilities for the k8s container orchestration platform.


=============
Grafana
=============

-> Grafana is an  analysis and monitoring tool.

-> It provides visulization for monitoring.

-> It provides charts, graphs, and alerts for the web when connected to supported data sources.

Note: Graphana will connect with Prometheus for data source.

===========================================
How to deploy Grafana & Prometheus in K8S
===========================================

-> Using HELM charts we can easily deploy Prometheus and Grafana

========================================================
Install Prometheus & Grafana In K8S Cluster using HELM
========================================================

# Add the latest helm repository in Kubernetes
$ helm repo add stable https://charts.helm.sh/stable


# Add prometheus repo to helm
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts


# Update Helm Repo
$ helm repo update


# install prometheus
$ helm install stable prometheus-community/kube-prometheus-stack


# Get all pods 
$ kubectl get pods

Node: You should see prometheus pods running


# Check the services 
$ kubectl get svc


# By default prometheus and grafana services are available within the cluster as ClusterIP, to access them outside lets change it to LoadBalancer.


# Edit Prometheus Service & change service type to LoadBalancer then save and close that file
$ kubectl edit svc stable-kube-prometheus-sta-prometheus

# Now edit the grafana service & change service type to LoadBalancer then save and close that file
$ kubectl edit svc stable-grafana

# Verify the service if changed to LoadBalancer
$ kubectl get svc


=> Access Promethues server using below URL

    URL : http://LBR-DNS:9090/

=> Access Grafana server using below URL

    URL : http://LBR-DNS/

=> Use below credentials to login into grafana server

UserName: admin
Password: prom-operator

=> Once we login into Grafana then we can monitor our k8s cluster. Grafana will provide all the data in charts format.


===============
Node Selector
===============

=> Node Selector is used to schedule the pods on particular worker node only.

=> To achieve this we can assign label for the worker node and we can configure that node label in our manifest yml as node selector.

Note: If node selector is matching with worker node label then our pods will be created on that particular worker node. If node-selector not matching with worker node label then pods will not be scheduled for execution.

=> Execute below manifest yml to create nginx deployment with 3 pod replicas

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      nodeSelector:
        name: ashokit-wn-1
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
...

$ kubectl apply -f <yml>

$ kubectl get pods

Note: here pods will be in pending state beause no worker node label is matching with node selector configured in manifest yml file.

# configure label for worker node
$ kubectl get nodes
$ kubectl edit node <node-name>

# configure below lable under labels section
name: ashokit-wn-1

$ kubectl get pods

Note: here pods will come into running state.

===============
Node Affinity
===============

=> Node Affinity preffered approach. If nodeSelector is matching with any worker node label then schedule pods on that worker node only. 

=> If matching is not found then schedule pods on any available worker node in the cluster.


---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      affinity:
       nodeAffinity:
        preferredDuringSchedulingIgnoredDuringExecution:
        - weight: 1
          preference:
           matchExpressions:
           - key: name
             operator: In
             values:
             - ashokit
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
...

$ kubectl apply -f <yml>

$ kubectl get pods -o wide


========
Taints 
========

=> Taints are used to make worker node not eligible for pods scheduling.

=> We have 3 popular taints

	1) No Schedule

	2) No Execute

	3) Prefer No Scedule

# create taint on worker node
$ kubectl taint nodes <node-name> color=blue:NoSchedule


# remove taint from worker node
$ kubectl taint nodes <node-name> key1=value1:NoSchedule-

============
Tolerations
============

=> Tolerations are used to schedule pods on tainted worker nodes also.

# tainting worker node
$ kubectl taint nodes <node-name> key1=value1:NoSchedule


Note: If any pod is having tolerations as key1 and value1 then schedule those pods even though the nodes are tainted with NoSchedule state.


---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      tolerations:
      - key: "key1"
        operator: "Equal"
        value: "value1"
        effect: "NoSchedule"
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
...

=========
PV & PVC
=========

PVC : Persistence Volume Claim (It is a request for storage)

PV : Persistence Volume

=> PVC is used to request storage for the PODS.
=> PVC will request for PV using storage classes.


=> Storage Classes provides diffrent types of sotages for PV

	Ex: efs, ebs, standard etc...

=> PV provides storage for the resources in k8s.

Note: PV and PVC we will use with StatefulSet.


---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 3
  minReadySeconds: 10
  template:
    metadata:
      labels:
        app: nginx
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: registry.k8s.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: standard
      resources:
        requests:
          storage: 1Gi
...          


===========================
Liveness & Readines Probes
===========================

Liveness Probe : It is used to determine POD is still running or not. If POD is not running then K8s will restart that pod based on  restart policy.

Readiness Probe : It is used to determine POD is ready to serve requests or not. If POD is not ready to serve the requests then k8s will remove that pod from load balancer.


---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        livenessProbe:
         httpGet:
          path: /health
          port: 80
         initialDelaySeconds: 5
        readinessProbe:
         httpGet:
          path: /ready
          port: 80
         initialDelaySeconds: 5
...









1) What is Orchestration
2) K8S introduction
3) K8S Advantages
4) K8S Architecture
5) K8S Setup (minikube & EKS)
6) What is POD
7) What is Service (ClusterIP, NodePort, LBR)
8) What is Namespace
9) ReplicationController (RC)
10) ReplicaSet (RS)
11) Deployment
12) DaemonSet
13) StatefulSet
14) ConfigMap and Secrets
15) Blue-Green Deployment
16) EFK Stack
17) HELM Charts
18) K8S Monitoring
19) Node Selector
20) Node Affinity
21) Taints & Tolerations
