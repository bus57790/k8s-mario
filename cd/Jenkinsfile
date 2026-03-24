FROM jenkins/jenkins:lts-jdk17

# Switch to root to update container
USER root

# Install wget
RUN apt-get update && apt-get install -y \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Install Trivy
RUN apt-get update && apt-get install -y wget gnupg \
    && wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor -o /usr/share/keyrings/trivy.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | tee /etc/apt/sources.list.d/trivy.list \
    && apt-get update \
    && apt-get install -y trivy \
    && rm -rf /var/lib/apt/lists/*

# Install kubeconform
RUN set -eux; \
    PLATFORM="$(uname | tr '[:upper:]' '[:lower:]')"; \
    # Replace 'latest' with a specific tag if version pinning is required.
    wget -q "https://github.com/yannh/kubeconform/releases/latest/download/kubeconform-${PLATFORM}-amd64.tar.gz"; \
    tar -xzf "kubeconform-${PLATFORM}-amd64.tar.gz"; \
    cp kubeconform /usr/local/bin/kubeconform; \
    chmod +x /usr/local/bin/kubeconform; \
    rm -f "kubeconform-${PLATFORM}-amd64.tar.gz"

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/kubectl

# Install AWS CLI
RUN apt-get update && apt-get install -y \
    awscli && \
    rm -rf /var/lib/apt/lists/*

# Install Docker
RUN apt-get update && \
    apt-get install -y \
    ca-certificates \
    curl \
    sudo && \
    curl -fsSL https://get.docker.com -o install-docker.sh && \
    sudo sh install-docker.sh && \
    sudo usermod -a -G docker jenkins

# Expose Jenkins port
EXPOSE 8080

# Switch back to Jenkins user
USER jenkins

