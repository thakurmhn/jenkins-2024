FROM ubuntu:22.04


# Make sure the package repository is up to date.
RUN apt-get update && \
    apt-get -qy full-upgrade && \
    apt-get install -qy wget git && \
    wget https://builds.openlogic.com/downloadJDK/openlogic-openjdk/17.0.11+9/openlogic-openjdk-17.0.11+9-linux-x64-deb.deb && \
    apt-get install -qy ./openlogic-openjdk-17.0.11+9-linux-x64-deb.deb && \
# Install a basic SSH server
    apt-get install -qy openssh-server && \
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd && \
# Install JDK 8 (latest stable edition at 2019-04-01)
    apt-get install -qy openjdk-8-jdk && \
# Install maven
    apt-get install -qy maven && \
# Cleanup old packages
    apt-get -qy autoremove && \
# Add user jenkins to the image
    adduser --quiet jenkins && \
# Set password for the jenkins user (you may want to alter this).
    echo "jenkins:letme1in" | chpasswd && \
    mkdir /home/jenkins/.m2 && \
    mkdir /home/jenkins/bin && \
    mkdir -p /home/jenkins/jdk/bin && \
    mkdir -p /jdk/bin

#ADD settings.xml /home/jenkins/.m2/
# Copy authorized keys
COPY authorized_keys /home/jenkins/.ssh/authorized_keys
RUN ln -s /usr/lib/jvm/openlogic-openjdk-17-hotspot-amd64/bin/java /home/jenkins/bin/java
RUN ln -s /usr/lib/jvm/openlogic-openjdk-17-hotspot-amd64/bin/java /home/jenkins/jdk/bin/java
RUN ln -s /usr/lib/jvm/openlogic-openjdk-17-hotspot-amd64/bin/java /jdk/bin/java

RUN chown -R jenkins:jenkins /home/jenkins/.m2/ && \
    chown -R jenkins:jenkins /home/jenkins/.ssh/ && \
    chown -R jenkins:jenkins /home/jenkins/bin/ && \
    chown -R jenkins:jenkins /home/jenkins/jdk 

RUN rm -f ./openlogic-openjdk-17.0.11+9-linux-x64-deb.deb
USER jenkins
ENV JAVA_HOME=/home/jenkins/jdk/bin
ENV PATH=/home/jenkins/jdk/bin:$PATH
# Standard SSH port
USER root
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
