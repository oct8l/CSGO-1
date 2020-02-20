###########################################################
# Dockerfile that builds a CSGO Gameserver
###########################################################
FROM oct8l/steamcmdbase:latest

LABEL maintainer="gitmail@oct8l.email"

ENV STEAMAPPID 740
ENV STEAMAPPDIR /home/steam/csgo-dedicated
ENV CSINSTDIR /csgo-install

# Create autoupdate config
# Add entry script & ESL config
# Remove packages and tidy up
RUN set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends --no-install-suggests \
		wget=1.20.1-1.1 \
		ca-certificates=20190110 \
	&& mkdir -p ${STEAMAPPDIR}/csgo \
	&& cd ${STEAMAPPDIR} \
	&& wget https://raw.githubusercontent.com/oct8l/CSGO-1/master/Standard/etc/entry.sh \
	&& chmod 755 ${STEAMAPPDIR}/entry.sh \
	&& cd ${STEAMAPPDIR}/csgo \
	&& { \
			echo '@ShutdownOnFailedCommand 1'; \
			echo '@NoPromptForPassword 1'; \
			echo 'login anonymous'; \
			echo 'force_install_dir ${CSINSTDIR}'; \
			echo 'app_update ${STEAMAPPID}'; \
			echo 'quit'; \
		} > ${STEAMAPPDIR}/csgo_update.txt \
	&& chown -R steam:steam ${STEAMAPPDIR} \
	&& apt-get remove --purge -y \
		wget \
	&& apt-get clean autoclean \
	&& apt-get autoremove -y \
	&& rm -rf /var/lib/apt/lists/*

ENV SRCDS_FPSMAX=999 \
	SRCDS_TICKRATE=128 \
	SRCDS_PORT=27015 \
	SRCDS_TV_PORT=27020 \
	SRCDS_CLIENT_PORT=27005 \
	SRCDS_MAXPLAYERS=14 \
	SRCDS_TOKEN=0 \
	SRCDS_RCONPW="changeme" \
	SRCDS_PW="" \
	SRCDS_STARTMAP="de_dust2" \
	SRCDS_REGION=255 \
	SRCDS_MAPGROUP="mg_active" \
	SRCDS_GAMETYPE=0 \
	SRCDS_GAMEMODE=1

USER steam

WORKDIR $STEAMAPPDIR

VOLUME $STEAMAPPDIR

ENTRYPOINT ${STEAMAPPDIR}/entry.sh

# Expose ports
EXPOSE 27015/tcp 27015/udp 27020/udp
