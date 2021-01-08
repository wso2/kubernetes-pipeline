# WSO2 Kubernetes Pipeline Helm Chart

## Chart Details

WSO2 Kubernetes Pipeline provides tools and a preconfigured pipeline used for continuous integration and deployment.
The setup is deployed on top of Kubernetes using Helm, which makes the processes of configuring, installing, scaling and upgrading, quite simple.

Following are the tools used to install and monitor the pipeline

- Jenkins: For continuous integration
- Spinnaker: For continuous deployment to Kubernetes
- ELK: For centralized logging
- Prometheus Operator: For monitoring deployments and visualization using Grafana

![Architecture Diagram](pipeline_architecture.jpg)

## Installation

### Prerequisites

- Install and set up Helm

- Install NGINX Ingress Controller.

### Deploy the CI/CD pipeline

1. Download the sample `values.yaml` of the desired product

    * [WSO2 Enterprise Integrator](samples/values-ei-pattern-1.yaml)
    * [WSO2 Identity and Access Management](samples/values-is-pattern-1.yaml)
    * [WSO2 API Microgateway](samples/values-mgw.yaml)
    * [WSO2 API Micro Integrator](samples/values-mi.yaml)

2. Replace the placeholders with their respective values in the `values.yaml`.

    - <WSO2_SUBSCRIPTION_USERNAME>
    - <WSO2_SUBSCRIPTION_PASSWORD>
    - <REGISTRY_USERNAME>
    - <REGISTRY_PASSWORD>
    - <REGISTRY_EMAIL>
    - \<EMAIL>
    - <GITHUB_USERNAME>
    - <GITHUB_PASSWORD>

3. Add the WSO2 Helm repository.

    ```
    helm repo add wso2 https://helm.wso2.com
    helm repo update
    ```

4. Install the pipeline Helm chart by referring to the updated `values.yaml` file.

    ```
    kubectl create namespace <NAMESPACE>
    helm install <RELEASE_NAME> wso2/kubernetes-pipeline -f values.yaml --namespace <NAMESPACE>
    ```

> In the following steps, `example.com` refers to the default domain name. 
> If the default host has been overridden, change the domain name accordingly.

5. Obtain the external IP (`EXTERNAL-IP`) of the Ingress resources by listing down the Kubernetes Ingresses.

    ```
    kubectl get ing -n <NAMESPACE>

    NAME                            HOSTS                       ADDRESS            PORTS       AGE
    <RELEASE_NAME>-grafana          grafana.example.com         <EXTERNAL_IP>       80          20m
    <RELEASE_NAME>-kibana           kibana.example.com          <EXTERNAL_IP>       80          20m
    <RELEASE_NAME>-spinnaker-deck   spinnaker.example.com       <EXTERNAL_IP>       80, 443     20m
    <RELEASE_NAME>-spinnaker-gate   gate.spinnaker.example.com  <EXTERNAL_IP>       80, 443     20m
    jenkins-ingress                 jenkins.example.com         <EXTERNAL_IP>       80, 443     20m
    ```

6. Add the above hosts as an entry in `/etc/hosts` as follows:

    ```
    <EXTERNAL_IP>  grafana.example.com kibana.example.com spinnaker.example.com jenkins.example.com
    ```

7. Navigate to the following URLs on any web browser:

    - https://jenkins.example.com
    - https://spinnaker.example.com
    - https://grafana.example.com
    - https://kibana.example.com

## Change domain

To customize the domain name override the host values as shown in the example below.

> `example.com` refers to the domain name

```yaml
jenkins:
  ingress:
    host: jenkins.example.com

spinnaker:
  ingress:
    host: spinnaker.example.com

  ingressGate:
    host: gate.spinnaker.example.com

kibana:
  ingress:
    hosts:
      - kibana.example.com

prometheus-operator:
  grafana:
    ingress:
      hosts:
      - grafana.example.com
```

## Using ingress with SSL/TLS

The WSO2 Kubernetes Pipeline resource uses the NGINX Ingress Controller maintained by Kubernetes. 
It is possible to use SSL/TLS by adding a certificate to be used with the ingress controller.
This could be done using the following methods
- [Default SSL certificate](#default-ssl-certificate)
- [Add individual certificates](#add-individual-certificates)

### Default SSL certificate

Refer to [NGINX Ingress Controller user guide](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#default-ssl-certificate) on how to configure a default certificate.

### Add individual certificates

To add individual certificates to the each ingress endpoint,

1. Create secrets for each endpoint containing the certificate and the private key as mentioned in the [NGINX Ingress Controller user guide](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#tls-secrets).
2. Add the following content to your values.yaml with the secret and hostname
 
 > Replace `example.com`, `my-tls-cert` with your domain name and secret name respectively.

```yaml
jenkins:
  ingress:
    host: jenkins.example.com
    tls:
      - secretName: my-tls-cert
        hosts:
          - example.com

spinnaker:
  ingress:
    host: spinnaker.example.com
    tls:
      - secretName: -tls
        hosts:
          - example.com
  ingressGate:
    host: gate.spinnaker.example.com
    tls:
     - secretName: -tls
       hosts:
         - example.com

kibana:
  ingress:
    hosts:
      - kibana.example.com
    tls:
      - hosts:
          - example.com
        secretName: my-tls-cert

prometheus-operator:
  grafana:
    ingress:
      hosts:
      - grafana.example.com
      tls:
        - hosts:
            - example.com
          secretName: my-tls-cert
```

## Access private Github repositories

Use of private repositories are recommended for WSO2 Kubernetes pipeline. 
While it is possible to use the basic credentials to authorize the pipeline to use these private repositories, we recommend the use of Github personal access tokens since it provides a more control on the level of access.

1. Create a personal access token as mentioned [here](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line).
2. Add the username and personal access token to the values.yaml as shown below.

```yaml
github:
  username: <GITHUB_USERNAME>
  password: <PERSONAL_ACCESS_TOKEN>
```

## Change credentials for Jenkins administrator

Jenkins starts up with an administrative user by default. 
The password for this account could be changed by overriding the values as shown below.

```yaml
jenkins:
  username: <JENKINS_USERNAME>
  password: <JENKINS_PASSWORD>
```

In addition to this, we need to configure Spinnaker to authenticate with Jenkins since it would be used to run tests.
This could be done by overriding the additional scripts section to change the default credentials indicated by `JENKINS_USERNAME` and `JENKINS_PASSWORD`.

```yaml
spinnaker:
  halyard:
    additionalScripts:
      create: true
      data:
        enable_ci.sh: |-
          echo "Configuring jenkins master"
          USERNAME="<JENKINS_USERNAME>"
          PASSWORD="<JENKINS_PASSWORD>"
          $HAL_COMMAND config ci jenkins enable
          echo $PASSWORD | $HAL_COMMAND config ci jenkins master edit master --address http://jenkins-service.{{ .Release.Namespace }}.svc.cluster.local:8080 --username $USERNAME --password || echo $PASSWORD | $HAL_COMMAND config ci jenkins master add master --address http://jenkins-service.{{ .Release.Namespace }}.svc.cluster.local:8080 --username $USERNAME --password
          $HAL_COMMAND config features edit --pipeline-templates true
```
