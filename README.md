<div align="center">
    <h1>
        <div>
            ISEC6000
        </div>
        <div>
            Assignment 1 Task 1
        </div>
        <div>
            Setup Kubernetes Cluster in Google Kubernetes Engine (GKE)
        </div>
    </h1>
</div>
This document outlines the steps required for setting up a Kubernetes Cluster on GKE and connecting kubectl on your local environment to the Kubernetes Cluster.

## Requirements
- Google Cloud Platform (GCP) Account
- [GCP Project](https://developers.google.com/workspace/guides/create-project)
- [GCloud CLI tool](https://cloud.google.com/sdk/docs/install#linux)
- [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

## Setting up Kubernetes Cluster on GKE
### 1. Navigate to Kubernetes Engine Cluster page
Navigate to the Kubernetes Engine Cluster page through the sidebar as show below.

![Screenshot 2023-09-03 214516](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/cbd6705f-81e8-45c8-bf20-c6766aeaefe0)

If Kubernetes Engine API has not been enabled for your project, you will need to do so through the [this link](https://console.cloud.google.com/apis/library/container.googleapis.com).

![Screenshot 2023-09-03 221539](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/a34443ab-5eac-477d-82e3-81588d967b98)

### 2. Create Kubernetes Cluster

In the kubernetes Cluster page click on "CREATE".

![Screenshot 2023-09-03 222414](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/8e5f1e9b-9d87-46a9-ae04-f71effabbcc6)

Enter the configuration and the settings for the Kubernetes Cluster in the create cluster page.
**Note:** An Autopilot Cluster is created for this project, therefore node pool configuration is not required.

![Screenshot 2023-09-03 223338](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/61dcd01c-9d38-4d5a-95bd-d0dda7857d44)

The following settings are used to create the Cluster for this project:
|**Setting**| **Value** |
|-----------|-----------|
|Name       |isec6000-assignment-1-cluster|
|Region     |australia-southeast1|
|Network    |default|
|Node Subnet|default|
|IPv4 network access| Public cluster|
|Pod IPv4 address range| /17|

Default "Advanced settings" are used in this project.

Proceed to reviewing and creating the cluster once the correct settings has been given to the cluster.

## Configuring and Connecting kubectl in Local Environment to GKE Cluster
### 1. Generating kubeconfig Entry
In GKE Clusters page, next to the cluster there is an option to connect to the cluster. Select the "Connect" option.
![Screenshot 2023-09-03 231618](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/f1514ae7-364d-433d-91bc-cdf3b219fb7c)

Copy the command line that appears in the pop up and run it in the terminal on your local environment. This will generate a kubeconfig entry in your local environment.
![Screenshot 2023-09-03 232438](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/303bc51d-e024-45a6-811c-c3247ebab50a)

### 2. Verify that Kubectl is connected to the Kubernetes Cluster
You can verify that kubectl is connected to the Kubernetes Cluster by running the following command:

```
kubectl config current-context
```

The output from the command will be in the following format:
```
gke_<project ID>_<region>_<cluster_name>
```
### 3. Test kubectl is properly configured by deploying hello-app
You can test that the kubectl has been properly configure to the Kubernetes cluster by deploying the hello-app and exposing the deployment.

Make sure gke-gcloud-auth-plugin is installed.
<details>
    <summary>gke-gcloud-aut-plugin installation instruction</summary>
 If it is not installed run the following command in your Linux local environment:
 
```
sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin
```
</details>
</br>

hello-app can be deployed using the following commands:
```
kubectl create deployment hello-server --image=us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
```

The app can be exposed with the following command:
```
kubectl expose deployment hello-server --type LoadBalancer --port 80 --target-port 8080 
 ```
 Run the following command to get the service's external IP address
 ```
 kubectl get service hello-server
 ```
 You can inspect your application through:
 >https://\<External IP>

Your application should look like this.

![Screenshot 2023-09-04 223718](https://github.com/tzesheng-curtin/isec6000-assignment1-task1/assets/143274010/3fa8101b-0691-480e-a1b6-ce819a883979)

 **Note**: Its recommend to delete the hello-server service to prevent GCP from charging you for running the service.
 This can be done with the following command:
 ```
 kubectl delete service hello-server
 ```
