#
# https://kasmweb.com/docs/latest/guide/custom_images.html
# https://github.com/Kong/insomnia/releases
#
# 2024-08-15 built and pushed image to docker.io marktmilligan/kasn:latest 8.2.0, kasm 1.15.0
# 2023-10-14 built and pushed image to docker.io marktmilligan/kasn:latest 8.2.0, kasm 1.12.0
# 2023-02-11 built and pushed image to docker.io marktmilligan/kasn:latest 2023.2.2, kasm 1.14.0
#

FROM kasmweb/desktop:1.15.0

USER root

COPY kasmvnc.yaml /etc/kasmvnc/kasmvnc.yaml
COPY vnc_startup.sh /dockerstartup/vnc_startup.sh
COPY kasm_default_profile.sh /dockerstartup/kasm_default_profile.sh
RUN cp -r /home/kasm-user /tmp/kasm-user
RUN apt-get update && apt-get install sudo
RUN echo "kasm-user ALL=(ALL) NOPASSWD:ALL" >>/etc/sudoers.d/nopasswd
RUN DEBIAN_FRONTEND=noninteractive make-ssl-cert generate-default-snakeoil --force-overwrite
RUN adduser kasm-user ssl-cert

# Install Insomnia
RUN wget -O wget -O /tmp/insomnia.deb https://github.com/Kong/insomnia/releases/download/core%409.3.3/Insomnia.Core-9.3.3.deb && \
    dpkg -i /tmp/insomnia.deb && \
    rm /tmp/insomnia.deb

# Install baseline packages
RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get install --yes \
      bash \
      curl \
      htop \
      man \
      python3 \
      python3-pip \
      software-properties-common \
      sudo \
      vim \
      wget \
      rsync

USER 1000