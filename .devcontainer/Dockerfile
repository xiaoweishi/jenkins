# [Choice] Java version (use -bullseye variants on local arm64/Apple Silicon): 11, 17, 11-bullseye, 17-bullseye, 11-buster, 17-buster
ARG JDK_VERSION=11
FROM mcr.microsoft.com/vscode/devcontainers/java:${JDK_VERSION}

ENV http_proxy=${PROXY}
ENV HTTP_PROXY=${PROXY}
ENV https_proxy=${PROXY}
ENV HTTPS_PROXY=${PROXY}

# [Option] Install Maven
ARG INSTALL_MAVEN="false"
ARG MAVEN_VERSION=""
# [Option] Install Gradle
ARG INSTALL_GRADLE="false"
ARG GRADLE_VERSION=""
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update -y && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends vim iputils-ping sudo

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

ARG USER=""
RUN userdel -r vscode \
    && groupadd --gid 1000 ${USER} \
    && useradd --uid 1000 --gid 1000 -s /bin/bash -m ${USER} \
    && printf "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && cp /root/.bashrc /home/${USER} \
    && printf "\nexport GPG_TTY=\$(tty)\n" >> /home/${USER}/.bashrc

USER ${USER}
