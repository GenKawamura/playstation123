FROM gen6scp/ubuntu-x11

USER root
WORKDIR /root

ARG DEBIAN_FRONTEND=noninteractive

ARG LABEL_VERSION

# Install tools
RUN apt-get update -qq \
 && apt-get install -qqy --no-install-recommends \
      software-properties-common 


# install dependencies and tools
#  * Reference: https://ubuntu-mate.community/t/epsxe-missing-libs/21749/3
RUN apt-get -yqq install libncurses5 libsdl1.2debian libsdl-ttf2.0-0 libnghttp2-14 librtmp1 libldap-2.4-2 dialog unzip

# download ePSXe
RUN cd /usr/games && wget http://www.epsxe.com/files/${LABEL_VERSION}.zip \
    && unzip ${LABEL_VERSION}.zip \
    && chmod +x epsxe_x64 && rm -v ${LABEL_VERSION}.zip \
    && ln -s /usr/games/epsxe_x64 /usr/local/bin/epsxe


# install libssl & libcurl3 locally by unpacking deb-package
RUN cd /tmp \
    && wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl1.0/libssl1.0.0_1.0.2n-1ubuntu5.5_amd64.deb \
    && apt -y install ./libssl1.0.0_1.0.2n-1ubuntu5.5_amd64.deb

RUN wget http://archive.ubuntu.com/ubuntu/pool/universe/c/curl3/libcurl3_7.58.0-2ubuntu2_amd64.deb -O /tmp/libcurl3.dep \
    && dpkg -x /tmp/libcurl3.dep /tmp/libcurl \
    && mv /tmp/libcurl/usr/lib/x86_64-linux-gnu/libcurl.so.4* /usr/games


# install dependencies
RUN apt-get update -qq \
 && apt-get install -qqy --no-install-recommends \
      libcap2-bin \
      locales \
 && rm -rf /var/lib/apt/lists/*


# create ePSXe directories
RUN for d in /games /data /bios /plugins; do \
      mkdir "$d"; \
      chgrp adm "$d"; \
      chmod 2775 "$d"; \
    done


# copy files to container
COPY epsxe-bootstrap /usr/local/bin/epsxe-bootstrap

# stepdown to the ubuntu user
USER ubuntu
WORKDIR /home/ubuntu

# link /data to ePSXe config
RUN mkdir -p /home/ubuntu/.epsxe \
 && ln -s /data /home/ubuntu/.epsxe

# export volumes
VOLUME /data
VOLUME /bios
VOLUME /plugins

# set directories to be chowned on container start
ENV USER_DIRS /data:/bios:/plugins${USER_DIRS:+:$USER_DIRS}

# set the secondary entrypoint
ENV ENTRYPOINT0 /usr/local/bin/epsxe-bootstrap

################################################################################
# labels
################################################################################

ARG LABEL_NAME
ARG LABEL_VERSION
ARG LABEL_USAGE
ARG LABEL_VENDOR
ARG LABEL_VCS_URL
ARG LABEL_VCS_REF

LABEL org.label-schema.name="$LABEL_NAME" \
  org.label-schema.version="$LABEL_VERSION" \
  org.label-schema.usage="$LABEL_USAGE" \
  org.label-schema.vendor="$LABEL_VENDOR" \
  org.label-schema.vcs-url="$LABEL_VCS_URL" \
  org.label-schema.vcs-ref="$LABEL_VCS_REF" \
  org.label-schema.schema-version="1.0"
