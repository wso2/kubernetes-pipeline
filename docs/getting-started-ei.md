## Getting Started with Kubernetes Pipeline for WSO2 Enterprise Integration

Setting up a basic pipeline for WSO2 Enterprise Integrator on Kubernetes is quick and simple.

You can set up a simple CI/CD pipeline for WSO2 Enterprise Integrator in two steps.

  - Create a Docker image for WSO2 Enterprise Integrator.

  - Deploy the CI/CD pipeline.

Before you begin to develop your pipeline, set up the following prerequisites in your Kubernetes cluster.

### Prerequisites

  - Install and set up Helm.

  - Install NGINX Ingress Controller Git release tag `nginx-0.22.0`.

### Create WSO2 Enterprise Integration Docker Image

The first step in developing the pipeline is to create a Docker image for WSO2 Enterprise Integrator on a private Docker registry.
This pipeline utilizes Docker Hub, as its private registry.

After creating the repository, pull the latest image from WSO2 and push it to the private registry.

- If you have a [WSO2 Subscription](https://wso2.com/subscription), log into the WSO2 Docker registry using your Subscription credentials.

    ```
    docker login docker.wso2.com
    docker pull docker.wso2.com/wso2ei-integrator:6.5.0
    docker tag docker.wso2.com/wso2ei-integrator:6.5.0 <DOCKER_ORGANIZATION>/wso2ei-integrator
    ```

> <DOCKER_ORGANIZATION> refers to the name of your organization in Docker Hub.

- If you do not have a WSO2 subscription use the Docker Hub image `wso2/wso2ei-integrator:6.5.0` instead.

    ```
    docker pull wso2/wso2ei-integrator:6.5.0
    docker tag wso2/wso2ei-integrator:6.5.0<DOCKER_ORGANIZATION>/wso2ei-integrator
    ```

> The WSO2 Enterprise Integration Docker image automatically fetches product updates on a weekly basis.
If you do not have a WSO2 Subscription account, you can sign up for a Free Trial Subscription [here](https://wso2.com/subscription).

Then, log into your organization on Docker Hub and push the Enterprise Integrator image.

```
docker login
docker push <DOCKER_ORGANIZATION>/wso2ei-integrator
```

### Deploy the CI/CD pipeline

1. Download the [sample values.yaml](../samples/values-ei-pattern-1.yaml) file and replace the placeholders with their respective values.

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

3. Install the pipeline Helm chart by pointing to the updated `values.yaml` file.

    ```
    helm install --name <RELEASE_NAME> wso2/kubernetes-pipeline -f values-ei-pattern-1.yaml --namespace <NAMESPACE>
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
