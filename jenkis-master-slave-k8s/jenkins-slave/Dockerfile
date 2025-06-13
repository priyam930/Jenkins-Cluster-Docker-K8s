FROM redhat/ubi9:latest
LABEL maintainer="priyam930"
# Basic utilities & SSH server
RUN dnf clean all && \
dnf -y update && \
dnf install -y \
dnf-plugins-core \
sudo \
git \
wget \
unzip \
yum-utils \
openssh \
openssh-server \
openssh-clients \
java-21-openjdk && \
dnf clean all
# Docker repo + Docker installation
RUN dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo && \
dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin \
docker-compose-plugin && \
dnf clean all
# Create Jenkins user with proper access
RUN useradd -m -s /bin/bash -d /home/jenkins jenkins && \
echo "jenkins:jenkins123" | chpasswd && \
echo 'jenkins ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && \
usermod -aG docker jenkins && \
mkdir -p /home/jenkins/.ssh && \
chown -R jenkins:jenkins /home/jenkins/.ssh && \
chmod 700 /home/jenkins/.ssh
# SSH Configuration
RUN ssh-keygen -A && \
mkdir -p /etc/ssh/sshd_config.d && \
echo "PubkeyAuthentication yes" > /etc/ssh/sshd_config.d/custom.conf && \
echo "AuthorizedKeysFile .ssh/authorized_keys" >> /etc/ssh/sshd_config.d/custom.conf && \
echo "PermitRootLogin no" >> /etc/ssh/sshd_config.d/custom.conf && \
echo "PasswordAuthentication no" >> /etc/ssh/sshd_config.d/custom.conf
# Set user and working directory
USER jenkins
WORKDIR /home/jenkins
# Expose ports: SSH (if needed)
EXPOSE 22
# Final CMD to launch both services
CMD ["bash", "-c", "sudo dockerd & sudo /usr/sbin/sshd -D"]