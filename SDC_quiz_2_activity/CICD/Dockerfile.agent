# Start from the Google Cloud SDK image, which already has kubectl
FROM google/cloud-sdk:latest

# Switch to the root user to install software
USER root

# Install the Docker CLI using the same method as before
RUN apt-get update && \
    apt-get install -y lsb-release curl gpg && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce-cli

# Clean up apt-get cache to keep the image small
RUN rm -rf /var/lib/apt/lists/*

# Switch back to a non-root user (optional but good practice)
USER nobody