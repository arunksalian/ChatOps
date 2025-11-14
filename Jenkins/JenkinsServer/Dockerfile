# Custom Jenkins Dockerfile (optional - uses official image as base)
FROM jenkins/jenkins:lts

# Switch to root to install additional packages
USER root

# Install Docker CLI (if you need Jenkins to run Docker commands)
RUN apt-get update && apt-get install -y \
    docker.io \
    && rm -rf /var/lib/apt/lists/*

# Install Git (required for Git plugin)
RUN apt-get update && apt-get install -y \
    git \
    && rm -rf /var/lib/apt/lists/*

# Switch back to jenkins user
USER jenkins

# Install Jenkins plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

