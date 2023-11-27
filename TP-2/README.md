# TP-2 - Deployment orchestation using Kubernetes #

* Manage, scale, and maintain containerized applications
  * Deployment in different environments
  * Scalability, deployment  ...

### Prerequisites strategies###

1. Minikube, available from [Kubernetes.io](https://kubernetes.io/fr/docs/tasks/tools/install-minikube/) and  the  `kubectl` command-line utility.
2. A virtualization provider to use with Minikube (docker)

## Part 1: Set up a local Kubernetes cluster

Run `minikube start --driver=docker` to have Minikube download an ISO image and boot a VM (using your installed virtualization provider) where it will run a single-node Kubernetes cluster.

Minikube will automatically create a configuration file that `kubectl` can use to connect to the Kubernetes cluster.

Kubernetes organizes all of its resources into namespaces by different projects or teams. You must create a namespace in which to do all the work required. 
To create a namespace `kubectl create namespace $NAMESPACE` and use it by default: `kubectl config set-context --current --namespace=NAMESPACE`.

## Part 2: Pods, Deployments, and Services

In this part, you'll use `kubectl` to create Kubernetes pods, deployments, and services without getting into any of the details around these concepts.

1. Use `kubectl create deployment nginx-web --image=nginx` to create a deployment witch launch an Nginx pod. This may take a few minutes as it must pull the Nginx container image down from the Internet.

2. Run `kubectl get pod,deployment` to see that the previous step created a deployment object as well as a single pod (managed by the deployment).

3. Run `kubectl describe deployment <deployment-name>` to get more details on the deployment created by step 1.

4. Run `kubectl describe pod <pod-name>` to get more details on the pod created by step 1.

5. Use `kubectl expose deployment nginx-web --name nginx-service --port=8888` to make this Nginx pod accessible outside the cluster.

6. Use `kubectl get pod,deployment,service` to see the objects created by the previous two steps. You should see a single deployment, a single pod, and a single service.

7. Run `kubectl describe service nginx-service` to get more details on the pod created by step 1. Note the "Selector" line, which shows that this service is using the "app=nginx-web" label to note the pods that will be part of this service.

8. Delete the service and the deployment using `kubectl delete service nginx-service` and `kubectl delete deployment nginx-web`, respectively. Use `kubectl get pods` to verify that deleting the deployment also deleted the pod.

## Part 3: Manage our backend and frontend application

In this final part, you'll examine more details on pods, deployments, and services, and work with the YAML files that define these objects in Kubernetes.

To do this, we're going to start again with our application used in the previous workshop:

we're using a private registry on docker hub to store versions of our images, so we'll need to think about creating a secret so that we can retrieve built images.

Type the following command:

```console
 kubectl create secret docker-registry efrei2023  --docker-server=docker.io  --docker-username=efrei2023 --docker-password=efrei2023 --docker-email=abdoulzak@proton.me
```

# Backend deployment

1. Examine the `backend-deployment.yaml` file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
  labels:
    name: backend
spec:
  selector:
    matchLabels:
      app: backend
  replicas: 1
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backend-container
          image: efrei2023/backend:1
          imagePullPolicy: Always
          ports:
            - containerPort: 5432
          env:
            - name: POSTGRES_PASSWORD
              value: postgres
            - name: POSTGRES_USER
              value: postgres
      imagePullSecrets:
        - name: efrei2023 
---
apiVersion: v1
kind: Service
metadata:
  name: backend-service
  labels:
    app: backend
spec:
  type: NodePort
  ports:
    - port: 5432
  selector:
    app: backend

```

3. Create the deployment by running `kubectl apply -f backend-deployment.yaml`. Use `kubectl get pods,deployments,services` to see that a deployment was created.

8. Use `kubectl get deployments,replicasets,pods` to see that a replica set was created automatically when you created the deployment.

# Frontend deployment

For the Deployment:

* The image needs to be `efrei2023/frontend:$VERSION`.
* The port exposed from the container should be **80**.
* Label the pods with `app: frontend`.

For the Service:

* The type of _Service_ should be `NodePort`.
* The service port should be **80**.
* The target port should be **8000**.
* Use the label `app` and the value `frontend` for the selector.

The YAML you can use, is provided below:

`frontend-deployment.yaml`:

<details markdown="1">
<summary>Click here for the frontend deployment YAML</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    name: frontend
spec:
  selector:
    matchLabels:
      app: frontend
  replicas: 1
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontend
          image: efrei2023/frontend:1
          ports:
            - name: http
              containerPort: 80
          imagePullPolicy: Always
      imagePullSecrets:
        - name: efrei2023
```

</details>
</br>

`frontend-service.yaml`:

<details markdown="1">
<summary>Click here for the frontend service YAML</summary>

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
  labels:
    name: frontend
spec:
  type: NodePort
  ports:
    - name: http
      port: 8000
      targetPort: http
  selector:
    app: frontend
```

</details>
</br>

## Accessing and Using the App

Once the two YAMLs have been applied:

* Access your application with `minikube service frontend-service`.

### Minikube dashboard overview ###

The Dashboard is a web-based Kubernetes user interface. You can use it to:

    Deploy containerized applications to a Kubernetes cluster
    Troubleshoot your containerized application
    Manage the cluster resources
    Get an overview of applications running on your cluster
    Creating or modifying individual Kubernetes resources (such as Deployments, Jobs, DaemonSets, etc)
