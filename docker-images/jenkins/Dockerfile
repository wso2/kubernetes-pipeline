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

FROM jenkins/jenkins:2.150.3
USER root

ARG HELM_VERSION=v2.14.1
ARG JEMETER_VERSION=5.1
ARG MGW_TOOLKIT_VERSION=3.0.1
ARG DOCKER_VERSION="18.06.3~ce~3-0~debian"
ARG MAVEN_VERSION=3.6.3
ARG MAVEN_SHA=c35a1803a6e70a126e80b2b3ae33eed961f83ed74d18fcd16909b2d44d7dada3203f1ffe726c17ef8dcca2dcaa9fca676987befeadc9b9f759967a8cb77181c0
ARG MAVEN_BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries

#Pre-Install Jenkins Plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Install Docker
RUN apt-get update && apt-get install software-properties-common apt-transport-https ca-certificates -y; \
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -;\
add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/debian  $(lsb_release -cs) stable";\
apt-get update && apt-get install docker-ce=${DOCKER_VERSION} -y

# Install maven
RUN mkdir -p /usr/share/maven \
  && curl -fsSL -o /tmp/apache-maven.tar.gz ${MAVEN_BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && echo "${MAVEN_SHA}  /tmp/apache-maven.tar.gz" | sha512sum -c - \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "/.m2"

# Install Helm
RUN curl https://raw.githubusercontent.com/helm/helm/${HELM_VERSION}/scripts/get > get_helm.sh && \
chmod 700 get_helm.sh && \
./get_helm.sh --version ${HELM_VERSION} && \
rm get_helm.sh

# Install Jmeter
RUN wget http://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JEMETER_VERSION}.tgz && \
tar -C /usr/local -xzf apache-jmeter-${JEMETER_VERSION}.tgz && \
rm apache-jmeter-${JEMETER_VERSION}.tgz

# Grant jenkins user group access to /var/run/docker.sock
RUN addgroup --gid 1001 dsock && \
gpasswd -a jenkins dsock && \
usermod -aG sudo jenkins

# Install microgateway toolkit
COPY wso2am-micro-gw-toolkit-${MGW_TOOLKIT_VERSION}.zip /wso2am-micro-gw-toolkit-${MGW_TOOLKIT_VERSION}.zip
RUN unzip /wso2am-micro-gw-toolkit-${MGW_TOOLKIT_VERSION}.zip && \
rm /wso2am-micro-gw-toolkit-${MGW_TOOLKIT_VERSION}.zip

ENV PATH="${PATH}:/wso2am-micro-gw-toolkit-${MGW_TOOLKIT_VERSION}/bin:/usr/local/apache-jmeter-${JEMETER_VERSION}/bin"
