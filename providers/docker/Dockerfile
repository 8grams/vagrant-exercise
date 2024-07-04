FROM ubuntu:22.04

# Install OpenSSH server
RUN apt-get update && \
    apt-get install -y openssh-server && \
    mkdir /var/run/sshd && \
    echo 'root:root' | chpasswd && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PermitRootLogin/PermitRootLogin/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Create the Vagrant user
RUN useradd -m -s /bin/bash vagrant && \
    echo 'vagrant:vagrant' | chpasswd && \
    mkdir -p /home/vagrant/.ssh && \
    chmod 0700 /home/vagrant/.ssh

# Add Vagrant public key
ADD vagrant.key.pub /home/vagrant/.ssh/id_rsa.pub
RUN cat /home/vagrant/.ssh/id_rsa.pub > /home/vagrant/.ssh/authorized_keys && \
    chmod 0600 /home/vagrant/.ssh/authorized_keys && \
    chown -R vagrant:vagrant /home/vagrant/.ssh

# Expose SSH port
EXPOSE 22

# Run SSH service
CMD ["/usr/sbin/sshd", "-D"]
