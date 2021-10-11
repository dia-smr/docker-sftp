FROM ubuntu:20.04

ARG SFTP_USER=sftp
ARG SFTP_PASS=sftp
ARG ACCESS_PASS=yes
ARG ACCESS_SSHKEY=yes

ENV SFTP_PASS=${SFTP_PASS}

RUN apt-get update \
&& DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
 -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" \
 mysecureshell whois procps openssh-server \
&& apt-get clean

RUN useradd -m -p $(mkpasswd -m sha-512 -s ${SFTP_PASS}) ${SFTP_USER} \
&& mkdir -p "/var/www/${SFTP_USER}/data/upload" \
&& mkdir /var/run/sshd \
&& chown ${SFTP_USER}:${SFTP_USER} -R /var/www/${SFTP_USER}/data/upload \
&& chown ${SFTP_USER}:${SFTP_USER} -R /home/${SFTP_USER} \
&& chmod 777 /var/www/${SFTP_USER}/data/upload \
&& echo "Match User ${SFTP_USER} \n\
        X11Forwarding no \n\
        PasswordAuthentication ${ACCESS_PASS} \n\
        PubkeyAuthentication ${ACCESS_SSHKEY} \n\
        AllowTcpForwarding no \n\
        AllowAgentForwarding no \n\
        PermitTunnel no \n\
        ForceCommand internal-sftp \n\
        ChrootDirectory /var/www/${SFTP_USER}/data \n\
">> /etc/ssh/sshd_config

CMD ["/usr/sbin/sshd", "-D"]
