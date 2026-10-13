Kubernetes MongoDB & Mongo Express

A beginner-friendly Kubernetes project that deploys MongoDB and Mongo Express using Kubernetes Deployments, Services, Secrets, and ConfigMaps.

This project demonstrates how to connect applications inside a Kubernetes cluster and access a web-based database management interface from a browser.

🛠️ Technologies Used

* Kubernetes — Container orchestration
* MongoDB 8 — NoSQL database
* Mongo Express — Web-based MongoDB management interface
* YAML — Kubernetes resource configuration
* Minikube — Local Kubernetes cluster
* kubectl — Kubernetes command-line tool

📂 Project Structure

kubernetes-mongodb-mongo-express/
├── configmap.yaml
├── secret.yaml
├── deployment.yaml
├── service.yaml
└── README.md

🏗️ Architecture

The project consists of two main applications:

* MongoDB: Stores the database and uses credentials supplied through a Kubernetes Secret.
* Mongo Express: Provides a web interface for viewing and managing MongoDB databases.

Kubernetes resources are used as follows:

* Deployment: Creates and manages MongoDB and Mongo Express Pods.
* ClusterIP Service: Allows Mongo Express to connect to MongoDB using the internal hostname mongo-service.
* NodePort Service: Exposes Mongo Express so it can be accessed through a web browser.
* Secret: Stores MongoDB authentication credentials.
* ConfigMap: Stores the MongoDB Service hostname.

Connection Flow

                 Web Browser
                      |
                      v
             NodePort Service
                 Port 30001
                      |
                      v
             Mongo Express Pod
                 Port 8081
                      |
                      v
             ClusterIP Service
                mongo-service
                 Port 27017
                      |
                      v
                MongoDB Pod
                 Port 27017

⚙️ Prerequisites

Before running this project, make sure you have installed:

* Docker
* Minikube
* kubectl

You can verify your installations using:

docker --version
minikube version
kubectl version --client

🚀 Getting Started

1. Clone the Repository

git clone https://github.com/YOUR_USERNAME/kubernetes-mongodb-mongo-express.git

Navigate to the project directory:

cd kubernetes-mongodb-mongo-express

Replace YOUR_USERNAME with your GitHub username.

2. Start Minikube

minikube start

Check the cluster status:

kubectl cluster-info
kubectl get nodes

3. Create the Kubernetes Secret

Apply the Secret configuration:

kubectl apply -f secret.yaml

The Secret provides the MongoDB root username and password to the MongoDB and Mongo Express containers.

Security note: The example credentials in this learning project are not suitable for production. Do not commit real credentials to a public repository.

4. Create the ConfigMap

kubectl apply -f configmap.yaml

The ConfigMap stores the MongoDB Service hostname:

data:
  database_url: mongo-service

Mongo Express uses this hostname to locate MongoDB inside the Kubernetes cluster.

5. Deploy MongoDB and Mongo Express

kubectl apply -f deployment.yaml

This creates the Deployments for both applications.

Check the Pods:

kubectl get pods

Wait until the Pods are running before continuing.

6. Create the Services

Both Services are defined in service.yaml, separated by the YAML document separator ---.

Apply them using:

kubectl apply -f service.yaml

Check the Services:

kubectl get services

The configuration includes:

Service	Type	Port	Purpose
mongo-service	ClusterIP	27017	Internal access to MongoDB
mongo-express-service	NodePort	30001	External access to Mongo Express

The NodePort Service selects Pods using the label app: mongo-express.

🌐 Access Mongo Express

After the Pods are running and the Services have been created, run:

minikube service mongo-express-service

Minikube should open Mongo Express in your browser.

Alternatively, use port forwarding:

kubectl port-forward service/mongo-express-service 8081:8081

Open the following address:

http://localhost:8081

Keep the port-forwarding command running while using the application.

Mongo Express Login

The default web authentication credentials may be:

* Username: admin
* Password: pass

If you configure custom web authentication credentials through environment variables, use those instead.

The Mongo Express web login is separate from the MongoDB database credentials stored in the Kubernetes Secret.

🔍 Useful Kubernetes Commands

Check Pods

kubectl get pods

Check Deployments

kubectl get deployments

Check Services

kubectl get services

Check ConfigMaps

kubectl get configmaps

Check Secrets

kubectl get secrets

View MongoDB Logs

kubectl logs deployment/mongo

View Mongo Express Logs

kubectl logs deployment/mongo-express

Describe a Pod

kubectl describe pod <pod-name>

Check MongoDB Service Endpoints

kubectl get endpoints mongo-service

🧹 Clean Up

To delete the resources created for this project, run:

kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
kubectl delete -f configmap.yaml
kubectl delete -f secret.yaml

These commands remove the Kubernetes resources defined in the corresponding files.

📚 What I Learned

Through this project, I practised:

* Creating Kubernetes Deployments.
* Exposing applications using ClusterIP and NodePort Services.
* Connecting Pods through Kubernetes Service discovery.
* Managing configuration using ConfigMaps.
* Passing credentials to containers using Secrets.
* Deploying multiple applications in one Kubernetes cluster.
* Accessing a Kubernetes application through a web browser.
* Inspecting Pods, Services, Deployments, and application logs.

🔮 Future Improvements

* Use StatefulSets and PersistentVolumes for MongoDB storage.
* Configure MongoDB replication for high availability.
* Improve credential management and security.
* Add readiness and liveness probes.
* Use resource requests and limits.
* Explore Helm for packaging and deploying Kubernetes applications.

👨‍💻 Author

Subash Maharjan

BSc. (Hons) Information Technology
Interested in AWS, Cloud Engineering, Kubernetes, and DevOps.

⸻

This project was created for learning and practising Kubernetes fundamentals.