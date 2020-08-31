# Copyright (c) 2019, WSO2 Inc. (http://www.wso2.org) All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM jenkins/jenkins:2.235.2

USER root

# Build arguments
ARG HELM_VERSION=v3.2.4
ARG JMETER_VERSION=5.1
ARG DOCKER_VERSION="18.06.3~ce~3-0~debian"
ARG MAVEN_VERSION=3.6.3
ARG MAVEN_SHA=c35a1803a6e70a126e80b2b3ae33eed961f83ed74d18fcd16909b2d44d7dada3203f1ffe726c17ef8dcca2dcaa9fca676987befeadc9b9f759967a8cb77181c0
ARG MAVEN_BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries
# Build arguments for WSO2 Microgateway Toolkit installation
ARG MGW_TOOLKIT_VERSION=3.2.0
ARG MGW_TOOLKIT_DIST_NAME=wso2am-micro-gw-toolkit-linux-${MGW_TOOLKIT_VERSION}
ARG MGW_TOOLKIT_DIST_URL=https://github.com/wso2/product-microgateway/releases/download/v${MGW_TOOLKIT_VERSION}/${MGW_TOOLKIT_DIST_NAME}.zip

# Install Docker
RUN \
    apt-get update \
    && apt-get install -y \
        apt-transport-https \
        ca-certificates \
        software-properties-common \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - \
    && add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/debian  $(lsb_release -cs) stable" \
    && apt-get update \
    && apt-get install -y docker-ce=${DOCKER_VERSION} \
    && rm -rf /var/lib/apt/lists/*

# Install Maven
RUN \
    mkdir -p /usr/share/maven \
    && curl -fsSL -o /tmp/apache-maven.tar.gz ${MAVEN_BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
    && echo "${MAVEN_SHA}  /tmp/apache-maven.tar.gz" | sha512sum -c - \
    && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
    && rm -f /tmp/apache-maven.tar.gz \
    && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

# Install Helm
RUN \
    wget https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz \
    && tar -zxvf helm-${HELM_VERSION}-linux-amd64.tar.gz \
    && mv linux-amd64/helm /usr/local/bin/helm \
    && rm helm-${HELM_VERSION}-linux-amd64.tar.gz \
    && rm -r linux-amd64

# Install JMeter
RUN \
    wget http://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz \
    && tar -C /usr/local -xzf apache-jmeter-${JMETER_VERSION}.tgz \
    && rm apache-jmeter-${JMETER_VERSION}.tgz

# Grant jenkins user group access to /var/run/docker.sock
RUN \
    addgroup --gid 1001 dsock \
    && gpasswd -a jenkins dsock \
    && usermod -aG sudo jenkins

# Copy the integration test Jenkins Job specification
COPY --chown=jenkins:jenkins config.xml /config.xml

# Install microgateway toolkit
RUN \
    wget --no-check-certificate "${MGW_TOOLKIT_DIST_URL}" \
    && unzip -d / ${MGW_TOOLKIT_DIST_NAME}.zip \
    && rm -f ${MGW_TOOLKIT_DIST_NAME}.zip

# Pre-Install Jenkins Plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

COPY --chown=root:root jenkins.sh /usr/local/bin/jenkins.sh

# Set environment variables
ENV MAVEN_CONFIG="/.m2" \
    MAVEN_HOME="/usr/share/maven" \
    PATH="${PATH}:/${MGW_TOOLKIT_DIST_NAME}/bin:/usr/local/apache-jmeter-${JMETER_VERSION}/bin"

USER jenkins
