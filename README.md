# HACluster WebApp Deployment

This project demonstrates the deployment of a simple web application using Docker and Kubernetes on a Linode Kubernetes cluster.

## Project Structure

```
├── webapp/
│   ├── index.html         # Web application HTML file
│   ├── Dockerfile         # Dockerfile to build the web application container
├── hacluster-deployment.yaml   # Kubernetes deployment manifest for HACluster
├── hacluster-service.yaml      # Kubernetes service manifest for HACluster
├── templates/
│   ├── deployment.yaml    # Template for Kubernetes deployment manifest
│   ├── service.yaml       # Template for Kubernetes service manifest
└── README.md              # Documentation
```

In docs you can find additional screenshots ande documentation.
The folder linodeToken contains the hacluster-kubeconfig.yaml from Linode and is not to be shared. 

## Prerequisites

- A Linode Kubernetes cluster.
- Docker installed locally for building the Docker image.
- Kubernetes CLI (`kubectl`) configured to manage your Linode Kubernetes cluster.
- Access to a container registry (e.g., Docker Hub or Linode Container Registry).

## Setup and Deployment

### Step 1: Build the Docker Image

1. Navigate to the `webapp` directory:
   ```bash
   cd webapp
   ```

2. Build the Docker image:
   ```bash
   docker build -t <your-registry-username>/hacluster:v1.0 .
   ```

3. Push the image to your container registry:
   ```bash
   docker push <your-registry-username>/hacluster:v1.0
   ```

### Step 2: Update Kubernetes Deployment File

1. Update the `image` field in `hacluster-deployment.yaml` with the correct image tag:
   ```yaml
   image: <your-registry-username>/hacluster:v1.0
   ```

### Step 3: Deploy to the Kubernetes Cluster

1. Apply the deployment manifest:
   ```bash
   kubectl apply -f hacluster-deployment.yaml
   ```

2. Apply the service manifest:
   ```bash
   kubectl apply -f hacluster-service.yaml
   ```

### Step 4: Verify the Deployment

1. Check the status of the deployment:
   ```bash
   kubectl get deployments
   ```

2. Check the status of the service:
   ```bash
   kubectl get services
   ```

3. Access the web application using the external IP provided by the service:
   ```bash
   kubectl get service hacluster-service
   ```

   Look for the `EXTERNAL-IP` field in the output and navigate to `http://<EXTERNAL-IP>` in your browser.

## Using the Templates Folder

The `templates` folder contains reusable Kubernetes manifest templates. These can be customized for other deployments.

- `deployment.yaml`: A template for Kubernetes deployments.
- `service.yaml`: A template for Kubernetes services.

## Notes

- The current setup includes a deployment with 3 replicas to ensure high availability.
- The default Docker image tag `v1.0` targets ARM architecture. You can update this to `v2.0` for x86 architecture or `latest` as needed.

## Cleanup

To remove the deployed resources, run:
```bash
kubectl delete -f hacluster-deployment.yaml
kubectl delete -f hacluster-service.yaml
```

## License

This project is open source and available under the [MIT License](LICENSE).

## Maintainer

milan.dima@vives.be