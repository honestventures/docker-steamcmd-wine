######## INSTALL ########

# Set the base image
FROM ubuntu:20.04

# Set Label
LABEL maintainer="repo@honestventures.biz"

# Set environment variables
ENV USER root
ENV HOME /root

# Set working directory
WORKDIR $HOME

# Insert Steam prompt answers
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN echo steam steam/question select "I AGREE" | debconf-set-selections \
 && echo steam steam/license note '' | debconf-set-selections

# Update the repository and install SteamCMD, libs, and Wine
ARG DEBIAN_FRONTEND=noninteractive
RUN dpkg --add-architecture i386 \
 && apt-get update -y \
 && apt-get install -y --no-install-recommends apt-utils \
 && apt-get install -y --no-install-recommends --no-install-suggests \
		locales \
		ca-certificates \
		nano \
		curl \
		wget \
		libc6 \
		libstdc++6 \
		lib32gcc1 \
		nuget \
		git \
		unzip \
	steamcmd \
			wine-stable \
			wine32 \
			wine64 \
			libwine \
			libwine:i386 \
			fonts-wine \
 && apt-get autoremove --purge -y \
 && rm -rf /var/lib/apt/lists/*

# Add unicode support
RUN locale-gen en_US.UTF-8
ENV LANG 'en_US.UTF-8'
ENV LANGUAGE 'en_US:en'

# Create symlink for executable
RUN ln -s /usr/games/steamcmd /usr/bin/steamcmd

## Make Directory for Running Game Servers with Pterodactyl Panel
RUN mkdir -p /mnt/server/steamcmd \
 && ln -s /usr/games/steamcmd /mnt/server/steamcmd/steamcmd.sh \
 && chown -R root:root /mnt

# Update SteamCMD and verify latest version
RUN steamcmd +quit

# Set default command
ENTRYPOINT ["steamcmd"]
CMD ["+help", "+quit"]
