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

FROM jenkins/jenkins:2.190.3
LABEL maintainer="WSO2 Docker Maintainers <dev@wso2.org>"

USER root

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
    && apt-get install docker-ce -y \
    && rm -rf /var/lib/apt/lists/*

# Install Helm
RUN \
    curl https://raw.githubusercontent.com/helm/helm/master/scripts/get > get_helm.sh \
    && chmod 700 get_helm.sh \
    && ./get_helm.sh \
    && rm get_helm.sh

# Grant jenkins user group access to /var/run/docker.sock
RUN \
    addgroup --gid 1001 dsock \
    && gpasswd -a jenkins dsock \
    && usermod -aG sudo jenkins

# Install microgateway toolkit
COPY wso2am-micro-gw-toolkit-3.0.1.zip /wso2am-micro-gw-toolkit-3.0.1.zip
RUN unzip /wso2am-micro-gw-toolkit-3.0.1.zip

ENV PATH="${PATH}:/wso2am-micro-gw-toolkit-3.0.1/bin"

#Pre-Install Jenkins Plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

USER jenkins
