# Changelog
All notable changes to this project 0.1.x per each release will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)

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
[v0.1.0](https://github.com/wso2/kubernetes-pipeline/milestone/1).

[v0.1.1]: https://github.com/wso2/kubernetes-pipeline/compare/v0.1.0...v0.1.1

[v0.1.2]: https://github.com/wso2/kubernetes-pipeline/compare/v0.1.1...v0.1.2
