# Changelog
All notable changes to this project per each release will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)

## [v1.1.0] - 2020-09-22

### Environments
- Successful evaluation of Kubernetes pipeline Helm chart in Google Kubernetes Engine (GKE) versions: `1.14.10-gke.50`, `v1.15.12-gke.2`

### Added
- Introduce Micro Integrator pipeline (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/66))
- Support For Test Scripts Stored In GitHub Private Repositories (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/78)) 
- Support For Pipeline Credential Updates (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/79)) 
- Micro integrator endpoint requirement at the docker build stage (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/95))
- Set Option to Define the Frequency of Registry Image Updates (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/76))
- Add Microgateway Toolkit to Jenkins Docker Image using a Downloadable Link (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/102))

### Changed
- Upgrade Base Jenkins Docker Image And Plugins Used (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/72))
- Change Jenkins deployment strategy to Recreate (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/98))
- Upgrade Stable Spinnaker Helm Chart Version (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/85))
- Use Helm Version 3 as the Rendering Engine in Spinnaker (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/83))
- Upgrade Elasticsearch Helm Chart Version (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/90))
- Upgrade Kibana Helm Chart Version (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/89))
- Upgrade Prometheus operator and Spinnaker charts (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/96))
- Upgrade Microgateway Toolkit Version to Latest (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/101))
- Dockerfile for Jenkins can be improved (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/7)) 

### Fixed
- Prometheus is not starting up (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/65)) 
- Helm chart build fails in Jenkins when chart contains no dependency (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/59))
- Define versions for the tools in the Jenkins image (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/68))
- Set Explicit Kubernetes Namespace Override By Spinnaker (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/77))
- Add artifact constraint for chart trigger (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/97))
- Fix the incorrect Docker repository in the sample MI value YAML (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/93))
- Micro integrator endpoint requirement at the docker build stage (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/95))
- Evaluate Usage of Latest Helm Version 2 in Jenkins (refer to [issue](https://github.com/wso2/kubernetes-pipeline/issues/82))


## [v1.0.0] - 2019-12-17

- Add advanced documentation topics
- Fix inconsistent grafana dashboard labels
- Change default hostnames to include example.com
- Upgrade dependency charts

## [v0.1.2] - 2019-11-04

### Changed

- Push latest tag when building microgateway docker image
- Remove shared library in casc configs
- Use automated pipeline triggers
- Tag each new Docker image with a timestamp in Microgateway
- Jenkins build fails when pushing Docker image for Microgateway
- Add a change log to this repository

## [v0.1.1] - 2019-09-02

### Changed

- Adding a missing selector in the Kubernetes Deployment specification for `v1`

## [v0.1.0] - 2019-08-31

### Added

- Add support for centralized logging using ELK
- Add support for monitoring using prometheus-operator
- Use Spinnaker Kubernetes deployment
- Configure Jenkins instance to support WSO2 Kubernetes Pipeline

For detailed information on the tasks carried out during this release, please see the GitHub milestone
[v1.0.0](https://github.com/wso2/kubernetes-pipeline/milestone/2).

[v0.1.1]: https://github.com/wso2/kubernetes-pipeline/compare/v0.1.0...v0.1.1

[v0.1.2]: https://github.com/wso2/kubernetes-pipeline/compare/v0.1.1...v0.1.2

[v1.0.0]: https://github.com/wso2/kubernetes-pipeline/compare/v0.1.2...v1.0.0

[v1.1.0]: https://github.com/wso2/kubernetes-pipeline/compare/v1.0.0...v1.1.0

