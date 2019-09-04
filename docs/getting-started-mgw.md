## Getting Started with Kubernetes Pipeline for WSO2 API Management

Setting up a basic pipeline for WSO2 API Microgateway on Kubernetes is quick and simple.

You can set up a simple CI/CD pipeline for WSO2 API Microgateway in two steps.

  - Create a Docker image for WSO2 API Microgateway.

  - Deploy the CI/CD pipeline.

Before you begin to develop your pipeline, set up the following prerequisites in your Kubernetes cluster.

### Prerequisites

  - Install and set up Helm.

  - Install NGINX Ingress Controller Git release tag `nginx-0.22.0`.

### Deploy the CI/CD pipeline

1. Download the [sample values.yaml](../samples/values-mgw.yaml) file and replace the placeholders with their respective values.

    - <WSO2_SUBSCRIPTION_USERNAME>
    - <WSO2_SUBSCRIPTION_PASSWORD>
    - <REGISTRY_USERNAME>
    - <REGISTRY_PASSWORD>
    - <REGISTRY_EMAIL>
    - \<EMAIL>
    - <GITHUB_USERNAME>
    - <GITHUB_PASSWORD>

2. Add the WSO2 Helm repository.

    ```
    helm repo add wso2 https://helm.wso2.com
    helm repo update
    ```

3. Install the pipeline Helm Chart by pointing to the updated `values.yaml` file.

    ```
    helm install --name <RELEASE_NAME> wso2/kubernetes-pipeline -f values-mgw.yaml --namespace <NAMESPACE>
    ```

4. Obtain the external IP (`EXTERNAL-IP`) of the Ingress resources by listing down the Kubernetes Ingresses.

    ```
    kubectl get ing -n <NAMESPACE>

    NAME                            HOSTS            ADDRESS            PORTS       AGE
    <RELEASE_NAME>-grafana          grafana         <EXTERNAL_IP>       80          20m
    <RELEASE_NAME>-kibana           kibana          <EXTERNAL_IP>       80          20m
    <RELEASE_NAME>-spinnaker-deck   spinnaker       <EXTERNAL_IP>       80, 443     20m
    <RELEASE_NAME>-spinnaker-gate   gate.spinnaker  <EXTERNAL_IP>       80, 443     20m
    jenkins-ingress                 jenkins         <EXTERNAL_IP>       80, 443     20m
    ```

5. Add the above hosts as an entry in `/etc/hosts` as follows:

    ```
    <EXTERNAL_IP>  grafana kibana spinnaker jenkins
    ```

6. Navigate to the following URLs on any web browser:

    - https://jenkins
    - https://spinnaker
    - https://grafana
    - https://kibana
