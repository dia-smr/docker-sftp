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
&& mkdir -p "/var/www/${SFTP_USER}/Upload" \
&& mkdir /var/run/sshd \
&& chown ${SFTP_USER}:${SFTP_USER} -R /var/www/${SFTP_USER}/Upload \
&& chown ${SFTP_USER}:${SFTP_USER} -R /home/${SFTP_USER} \
&& echo "Match User ${SFTP_USER} \n\
        X11Forwarding no \n\
        PasswordAuthentication ${ACCESS_PASS} \n\
        PubkeyAuthentication ${ACCESS_SSHKEY} \n\
        AllowTcpForwarding no \n\
        AllowAgentForwarding no \n\
        PermitTunnel no \n\
        ForceCommand internal-sftp \n\
        ChrootDirectory /var/www/${SFTP_USER} \n\
">> /etc/ssh/sshd_config
# USER ${SFTP_USER}
WORKDIR /var/www/${SFTP_USER}/Upload

EXPOSE 20 21 22

CMD ["/usr/sbin/sshd", "-D"]