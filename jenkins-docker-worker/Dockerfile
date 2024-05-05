# Centos9-stream created from scratch 
From thakurmhn/my_centos:centos9-stream

RUN dnf update -y && dnf install -y openssh-server passwd openssh wget
RUN useradd jenkins && \
	echo 'Letme1in' | passwd jenkins --stdin && \
	mkdir /home/jenkins/.ssh && \
	chmod 700 /home/jenkins/.ssh && \
        chown -R jenkins:jenkins /home/jenkins/.ssh/
COPY authorized_keys /home/jenkins/.ssh/authorized_keys
RUN chown jenkins:jenkins /home/jenkins/.ssh/authorized_keys && \
	chmod 700 /home/jenkins/.ssh/authorized_keys
# RUN pip install awscli 'ansible==8.5.0'


# Make sure that jdk/jre/java version should be same as java version on Jenkins master or agent will not show online
RUN wget https://builds.openlogic.com/downloadJDK/openlogic-openjdk/17.0.11+9/openlogic-openjdk-17.0.11+9-linux-x64-el.rpm && \
dnf install openlogic-openjdk-17.0.11+9-linux-x64-el.rpm -y

# RUN dnf install maven -y && chown -R jenkins:jenkins /home/jenkins/.m2/  
USER root
# generate ssh host keys
RUN ssh-keygen -A
CMD /usr/sbin/sshd -D
