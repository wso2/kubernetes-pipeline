# WSO2 Kubernetes Pipeline Helm Chart

## Chart Details

- WSO2 Kubernetes Pipeline provides tools and a preconfigured pipeline used for continuous integration and deployment.
The setup is deployed on top of Kubernetes using Helm, which makes the processes of configuring, installing, scaling and upgrading, quite simple.

- Following are the tools used to install and monitor the pipeline

  - Jenkins: For continuous integration
  - Spinnaker: For continuous deployment to Kubernetes
  - ELK: For centralized logging
  - Prometheus Operator: For monitoring deployments and visualization using Grafana

![Architecture Diagram](pipeline_architecture.jpg)

## Installation

Use the following, **Getting Started** guides to install the Kubernetes Pipeline, for the respective WSO2 product.

* [Getting Started with Kubernetes Pipeline for WSO2 Enterprise Integration](docs/getting-started-ei.md)

* [Getting Started with Kubernetes Pipeline for WSO2 Identity and Access Management](docs/getting-started-is.md)

* [Getting Started with Kubernetes Pipeline for WSO2 API Management](docs/getting-started-mgw.md)

## Change domain

To customize the domain name override the host values as shown in the example below.

> example.com refers to the domain name

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
