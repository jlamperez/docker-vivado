FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

ARG INSTALL_FILE="Xilinx_Unified_2021.2_1021_0703.tar.gz"

# Install language
RUN apt-get update && apt-get install -y \
  locales \
  && locale-gen en_US.UTF-8 \
  && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
  && rm -rf /var/lib/apt/lists/*
ENV LANG en_US.UTF-8

# Install timezone
RUN ln -fs /usr/share/zoneinfo/UTC /etc/localtime \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y tzdata \
  && dpkg-reconfigure --frontend noninteractive tzdata \
  && rm -rf /var/lib/apt/lists/*

RUN \
  sed -i -e "s%http://[^ ]\+%http://ftp.jaist.ac.jp/pub/Linux/ubuntu/%g" /etc/apt/sources.list && \
  apt-get update -y && \
  apt-get upgrade -y && \
  apt-get -y --no-install-recommends install \
    ca-certificates curl sudo xorg dbus dbus-x11 ubuntu-gnome-default-settings gtk2-engines \
    ttf-ubuntu-font-family fonts-ubuntu-font-family-console fonts-droid-fallback lxappearance gnupg gnupg2 && \
  apt-get autoclean && \
  apt-get autoremove && \
  rm -rf /var/lib/apt/lists/* && \
  echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

ARG gosu_version=1.14
RUN \
  curl -SL "https://github.com/tianon/gosu/releases/download/${gosu_version}/gosu-$(dpkg --print-architecture)" \
    -o /usr/local/bin/gosu && \
  curl -SL "https://github.com/tianon/gosu/releases/download/${gosu_version}/gosu-$(dpkg --print-architecture).asc" \
    -o /usr/local/bin/gosu.asc && \
  gpg --keyserver keyserver.ubuntu.com --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 && \
  gpg --verify /usr/local/bin/gosu.asc && \
  rm -rf /usr/local/bin/gosu.asc /root/.gnupg && \
  chmod +x /usr/local/bin/gosu

# vitis
RUN \
  dpkg --add-architecture i386 && \
  apt-get update && \
  apt-get -y --no-install-recommends install \
    autoconf automake bison build-essential cpio cython debianutils flex g++ gawk \
    gcc gcc-multilib git google-perftools haveged iproute2 iputils-ping \
    lib32stdc++6 libc6-dev:i386 libegl1-mesa liberror-perl libfontconfig1:i386 libgtk2.0-0:i386 \
    libjpeg62-dev libncurses-dev libsdl1.2-dev libsm6:i386 libssl-dev libtool libx11-6:i386 \
    libxcb-randr0-dev libxcb-shape0-dev libxcb-xinerama0-dev libxcb-xkb-dev libxcb-xtest0-dev \
    libxext6:i386 libxrender1:i386 mtd-utils net-tools ocl-icd-libopencl1 \
    ocl-icd-opencl-dev opencl-headers openssh-server openssl pax perl putty pylint python3 \
    python3-git python3-jinja2 python3-pexpect python3-pip screen sysvinit-utils texinfo \
    tftpd-hpa util-linux xinetd xterm xtrans-dev xz-utils zlib1g-dev zlib1g:i386 && \
  apt-get -y -f install && \
  apt-get autoclean && \
  apt-get autoremove && \
  rm -rf /var/lib/apt/lists/*

COPY install_config.txt /vitis-installer/
COPY ${INSTALL_FILE} /vitis-installer/

RUN \
  cat /vitis-installer/${INSTALL_FILE} | tar zx --strip-components=1 -C /vitis-installer && \
  /vitis-installer/xsetup -a XilinxEULA,3rdPartyEULA -b Install -c /vitis-installer/install_config.txt && \
  rm -rf /vitis-installer


COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

CMD ["/bin/bash", "-l"]
