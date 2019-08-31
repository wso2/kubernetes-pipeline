# Helm Resources for WSO2 Kubernetes Pipeline
*Helm Resources for container-based deployments of WSO2 Kubernetes Pipeline for WSO2 products*

## Overview
WSO2 Kubernetes pipeline provides tools and preconfigured pipelines used for continuous integration and deployment. The setup is deployed on top of Kubernetes using Helm, which makes the processes of configuring, installing, scaling and upgrading simple.

Following are the tools used to install and monitor the pipeline
- Jenkins: Continous integration
- Spinnaker: Continous deployment to Kubernetes
- ELK: Centralized logging
- Prometheus-operator: Monitoring deployments and visualization using grafana


![Architecture Diagram](pipeline_architecture.jpg)

## Installation

Use the following getting started guides to install Kubernetes pipelines for the resepective WSO2 product.

* [Getting started with WSO2 Enterprise Integrator](docs/getting-started-ei.md)
* [Getting started with WSO2 Identity and Access Management](docs/getting-started-is.md)
* [Getting started with WSO2 API Microgateway](docs/getting-started-mgw.md)

## Contact us

WSO2 developers can be contacted via the following mailing lists:

* WSO2 Developers Mailing List : [dev@wso2.org](mailto:dev@wso2.org)
* WSO2 Architecture Mailing List : [architecture@wso2.org](mailto:architecture@wso2.org)