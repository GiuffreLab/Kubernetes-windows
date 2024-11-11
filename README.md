# Windows Containers for Testing on Kubernetes

This project sets up a Windows container environment in Kubernetes for testing purposes, using Kustomize to manage Kubernetes resources. It includes Persistent Volume Claims, Deployments, Services, and a Namespace for Windows version 11.

The containerized environment includes RDP (Remote Desktop Protocol) access, allowing you to remotely access the Windows instance through your browser or other remote desktop clients.

## How to Deploy

To deploy the environment, you will need to use the `kustomization.yaml` file, which ties together all the necessary Kubernetes components, including namespace creation, storage provisioning, deployment, and services.

To deploy, follow these steps:

1. Clone the repository.
2. Navigate to the project directory.
3. Run the following command to apply the deployment:

   ```sh
   kubectl apply -k .
   ```

This will deploy all resources defined in the kustomization, including the Windows container with the appropriate namespace and volume.

## Accessing the Windows Container

### Browser Access via NodePort

After deployment, the Windows container can be accessed via a browser using the node port that is exposed by the service. For example, if your Kubernetes node IP address is `192.168.49.2`, you can access the container by going to:

```
http://192.168.49.2:30011
```

This will direct you to the application running on port `8006` inside the container.

### RDP Access via NodePort

The container also exposes the RDP service on port `3389`, allowing you to access the Windows container via Remote Desktop clients. You can find the NodePort associated with RDP using the following command:

```sh
kubectl get svc windows -n windows-11
```

Look for the NodePort assigned to `port 3389`. By default, this NodePort is `30011`. You can use this NodePort to connect via an RDP client to your Kubernetes node.

## Kubernetes Resources

The Kubernetes resources used in this project include:

- **Persistent Volume Claim (PVC)**: `pvc.yaml` defines a PVC to allocate storage for the Windows container.
- **Namespace**: `namespace.yaml` defines the namespace (`windows-11`) for isolating the resources.
- **Deployment**: `deployment.yaml` defines the Windows container, specifying environment variables, resource limits, and volume mounts.
- **Service**: `service.yaml` exposes the container via a NodePort service for both HTTP and RDP access.
- **Kustomization**: `kustomization.yaml` is used to manage and deploy the above resources cohesively.

## Original Project

This Kubernetes setup utilizes the Docker image created by the project available at [Dockurr Windows GitHub](https://github.com/dockur/windows). Visit the repository for more details about the container image itself.

