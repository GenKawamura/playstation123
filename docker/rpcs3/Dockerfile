FROM gen6scp/ubuntu-x11

USER root
WORKDIR /root

ARG DEBIAN_FRONTEND=noninteractive

ARG LABEL_VERSION

# Install tools
RUN apt-get update -qq \
 && apt-get install -qqy --no-install-recommends \
      software-properties-common

# install dependencies
RUN apt-get update -qq \
 && apt-get install -qqy --no-install-recommends \
       locales fuse kmod \
       && apt-get -qqy remove qtchooser && apt-get -qqy install libqt5gui5 \
       && rm -rf /var/lib/apt/lists/*
	      


# install dependencies and tools
#  Reference: https://linuxconfig.org/how-to-install-rpcs3-emulator-and-play-ps3-games-on-linux
# download RPCS3
RUN cd /usr/games && wget -q https://github.com/RPCS3/rpcs3-binaries-linux/releases/download/build-6357b9a04f46682e5fcdfb60aabe81ee97db48e0/${LABEL_VERSION}.AppImage \
    && chmod +x ${LABEL_VERSION}.AppImage \
    && ln -s /usr/games/${LABEL_VERSION}.AppImage /usr/local/bin/rpcs3


# create ePSXe directories
RUN for d in /games /data /bios /plugins; do \
      mkdir "$d"; \
      chgrp adm "$d"; \
      chmod 2775 "$d"; \
    done


# copy files to container
COPY rpcs3-bootstrap /usr/local/bin/rpcs3-bootstrap

# stepdown to the ubuntu user
USER ubuntu
WORKDIR /home/ubuntu

# link /data to PCSX2 config
RUN mkdir -p /home/ubuntu/.rpcs3 \
 && ln -s /data /home/ubuntu/.rpcs3

# export volumes
VOLUME /data
VOLUME /bios
VOLUME /plugins

# set directories to be chowned on container start
ENV USER_DIRS /data:/bios:/plugins${USER_DIRS:+:$USER_DIRS}

# set the secondary entrypoint
ENV ENTRYPOINT0 /usr/local/bin/rpcs3-bootstrap

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
