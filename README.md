#Documentation

Build the Docker image and push it to a container registry

docker build -t nodejs .

Runs the nodejs image in a detached mode (-d), mapping port 3000 inside the container to port 3000 on the host (-p 3000:3000)

docker run -p 3000:3000 nodejs

Create a Deployment named nodejsapp with three replicas running your Node.js application.

kubectl apply -f deployment.yaml

Use the kubectl apply -f crd.yaml command to apply the CRD YAML to the Kubernetes cluster

kubectl get crd ---> This will list all the CRDs in your cluster, including the newly installed

Step 1: Enable Metrics Server

Make sure Metrics Server is installed and running in the Kubernetes cluster. If it's not already installed, you can install it using the following command:

kubectl apply -f https://github.com/kubernetes-sigs/metricsserver/releases/latest/download/components.yaml

Step 2: Write HPA Configuration

Write an HPA configuration YAML file defining autoscaling behavior based on CPU or memory usage thresholds.

Step 3: Apply HPA Configuration

Apply the HPA configuration YAML using the kubectl apply -f myhpa.yaml

Stress Testing

Step 1: Define Resource Limits

Update the Deployment YAML to specify resource requests and limits for CPU and memory.

Apply the updated Deployment YAML to your Kubernetes cluster:

kubectl apply -f updeployment.yaml

Now for stress testing use hey tool:

for sending 1000 requests with 10 concurrent requests to http://loccalhost:3000

Use command:

hey -n 1000 -c 10 http://localhost:3000

Success Criteria:

● A functional Kubernetes deployment for the Node.js application.

● HPA configuration that automatically scales the application based on resource usage.

● Successful execution of the load test with a recorded screencast demonstrating scaling behavior.#Documentation

____________________________

Steps to Install Prometheus:
____________________________

Add Prometheus Helm repository: 

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

Update Helm repositories: 

helm repo update

Install Prometheus using Helm: 

helm install prometheus prometheus-community/prometheus

Expose Prometheus service to access externally: 

kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext

Access Prometheus dashboard: 

minikube service prometheus-server-ext

_________________________

Steps to Install Grafana:
_________________________

Add Grafana Helm repository: 

helm repo add grafana https://grafana.github.io/helm-charts

Update Helm repositories:

helm repo update

Install Grafana using Helm: 

helm install grafana grafana/grafana

Expose Grafana service to access externally: 

kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext

Access Grafana dashboard: 

minikube service grafana-ext
-----------------------------------------------
To get username and password in Grafana:

Retrieve Grafana secret in default namespace: 

kubectl get secret --namespace default grafana -o yaml

Decode the password: 

echo "secret code from yaml" | openssl base64 -d ; echo

_______________________________________________

Install Postgres Database on Kubernetes Cluster
_______________________________________________

Add the Bitnami Helm repository:

helm repo add bitnami https://charts.bitnami.com/bitnami

Installs PostgreSQL on the Kubernetes cluster using Helm, creating a deployment named "postgresql-dev" from the Bitnami PostgreSQL chart:

helm install postgresql-dev bitnami/postgresql

Retrieves the YAML representation of the secret named "postgresql-dev" :

kubectl get secret/postgresql-dev -oyaml

Decodes to reveal the PostgreSQL password

echo Tb2YzCppj8 | base64 -d:

Connect to the PostgreSQL database running on localhost (127.0.0.1) port 5432 using the username "postgres" and the decoded password, allowing interaction with the "postgres" database:

PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432

----------------------------------
For Data Persistence:

Deploy the below given deployment yaml in the Kubernetes cluster by using -------> for ex: kubectl apply -f grafana-pv.yaml

(grafana-pv.yaml, grafana-pvc.yaml, prometheus-pv.yaml, prometheus-pvc.yaml, postgres-pv.yaml, postgres-pvc.yaml)

These YAML files are used to provision and manage persistent storage for Grafana, Prometheus, and PostgreSQL deployments in a Kubernetes cluster, ensuring that data persists even if pods are restarted.
