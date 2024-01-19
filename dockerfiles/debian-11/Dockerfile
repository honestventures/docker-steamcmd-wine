############################################################
# Dockerfile that contains SteamCMD
############################################################
#FROM debian:bullseye-slim as build_stage
FROM debian:11-slim

LABEL maintainer="repo@honestventures.biz"
#ARG PUID=1000

# Set environment variables
ENV USER root
ENV HOME /root
#ENV USER steam
#ENV HOMEDIR "/home/${USER}"

# Set working directory
WORKDIR $HOME
#ENV STEAMCMDDIR "${HOMEDIR}/steamcmd"

# Insert Steam prompt answers
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN echo steam steam/question select "I AGREE" | debconf-set-selections \
 && echo steam steam/license note '' | debconf-set-selections

# Update the repository install 32-bit support
ARG DEBIAN_FRONTEND=noninteractive
COPY sources.list /etc/apt/sources.list
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
		lib32gcc-s1 \
		nuget \
		git \
		unzip \
	steamcmd \
#			wine-stable \
			wine \
			wine32 \
			wine64 \
			libwine \
			libwine:i386 \
			fonts-wine \
 && apt-get upgrade -y \
 && apt-get install -y tzdata \
 && apt-get install sudo -y \
 && apt-get autoremove --purge -y \
 && apt-get autoclean -y \
 && rm -rf /var/lib/apt/lists/*


#RUN set -x \
#	# Install, update & upgrade packages
#	&& apt-get update \
#	&& apt-get install -y --no-install-recommends --no-install-suggests \
#		lib32stdc++6=10.2.1-6 \
#		lib32gcc-s1=10.2.1-6 \
#		ca-certificates=20210119 \
#		nano=5.4-2+deb11u2 \
#		curl=7.74.0-1.3+deb11u11 \
#		locales=2.31-13+deb11u7 \
#	&& sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
#	&& dpkg-reconfigure --frontend=noninteractive locales \
#	# Create unprivileged user
#	&& useradd -u "${PUID}" -m "${USER}" \
#	# Download SteamCMD, execute as user
#	&& su "${USER}" -c \
#		"mkdir -p \"${STEAMCMDDIR}\" \
#		&& curl -fsSL 'https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz' | tar xvzf - -C \"${STEAMCMDDIR}\" \
#                && \"./${STEAMCMDDIR}/steamcmd.sh\" +quit \
#                && ln -s \"${STEAMCMDDIR}/linux32/steamclient.so\" \"${STEAMCMDDIR}/steamservice.so\" \
#                && mkdir -p \"${HOMEDIR}/.steam/sdk32\" \
#                && ln -s \"${STEAMCMDDIR}/linux32/steamclient.so\" \"${HOMEDIR}/.steam/sdk32/steamclient.so\" \
#                && ln -s \"${STEAMCMDDIR}/linux32/steamcmd\" \"${STEAMCMDDIR}/linux32/steam\" \
#                && mkdir -p \"${HOMEDIR}/.steam/sdk64\" \
#                && ln -s \"${STEAMCMDDIR}/linux64/steamclient.so\" \"${HOMEDIR}/.steam/sdk64/steamclient.so\" \
#                && ln -s \"${STEAMCMDDIR}/linux64/steamcmd\" \"${STEAMCMDDIR}/linux64/steam\" \
#                && ln -s \"${STEAMCMDDIR}/steamcmd.sh\" \"${STEAMCMDDIR}/steam.sh\"" \
#	# Symlink steamclient.so; So misconfigured dedicated servers can find it
#	&& ln -s "${STEAMCMDDIR}/linux64/steamclient.so" "/usr/lib/x86_64-linux-gnu/steamclient.so" \
#	# Clean up
#	&& apt-get remove --purge --auto-remove -y \
#	&& rm -rf /var/lib/apt/lists/*

# Add unicode support
##RUN locale-gen en_US.UTF-8
RUN sed -i 's/^# *\(en_US.UTF-8\)/\1/' /etc/locale.gen \
 && locale-gen en_US.UTF-8
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

# Fix missing directories and libraries
RUN mkdir -p $HOME/.steam \
 && ln -s $HOME/.local/share/Steam/steamcmd/linux32 $HOME/.steam/sdk32 \
 && ln -s $HOME/.local/share/Steam/steamcmd/linux64 $HOME/.steam/sdk64 \
 && ln -s $HOME/.steam/sdk32/steamclient.so $HOME/.steam/sdk32/steamservice.so \
 && ln -s $HOME/.steam/sdk64/steamclient.so $HOME/.steam/sdk64/steamservice.so

# Set default command
ENTRYPOINT ["steamcmd"]
CMD ["+help", "+quit"]

#FROM build_stage AS bullseye-root
#WORKDIR ${STEAMCMDDIR}

#FROM bullseye-root AS bullseye

# Switch to user
#USER ${USER}
