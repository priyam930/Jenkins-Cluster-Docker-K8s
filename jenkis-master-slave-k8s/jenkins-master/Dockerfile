FROM ubuntu:22.04

USER root

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    openjdk-17-jdk \
    openssh-client\
    wget \
    gnupg2 \
    ca-certificates \
    && wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key \
    && echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" > /etc/apt/sources.list.d/jenkins.list \
    && apt-get update && apt-get install -y jenkins \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

EXPOSE 8080 50000

CMD ["/usr/bin/java", "-jar", "/usr/share/java/jenkins.war"]



