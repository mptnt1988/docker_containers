FROM jenkins/jenkins:lts

USER root
# Install packages
RUN apt-get update
RUN apt-get install -y vim
# Prepare conda
RUN wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh
RUN mkdir -p /app/local/
RUN chown -R jenkins: /app/local/ /tmp/miniconda.sh
# Self-signed SSL
COPY ssl/ca.crt /usr/local/share/ca-certificates/ca.crt
RUN update-ca-certificates

USER jenkins
# Install Jenkins plugins
COPY jenkins/plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
# Install conda
RUN chmod +x /tmp/miniconda.sh
RUN /tmp/miniconda.sh -f -b -p /app/local/miniconda
# Configure
COPY jenkins/jenkins_bashrc /var/jenkins_home/.bashrc