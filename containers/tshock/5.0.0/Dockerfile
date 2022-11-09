FROM mono:slim

# Update and install needed utils
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y curl nuget vim zip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create symbolic link to ServerLog.txt
RUN mkdir /config /tshock && \
    touch /config/ServerLog.txt && \
    ln -s /config/ServerLog.txt /tshock/ServerLog.txt && \
    rm -rf /config

# fix for favorites.json error
RUN favorites_path="/root/My Games/Terraria" && mkdir -p "$favorites_path" && echo "{}" > "$favorites_path/favorites.json"

# Download and install TShock Server
# ENV TSHOCK_VERSION=5.0.0

RUN cd /tmp 
RUN curl -sL https://github.com/Pryaxis/TShock/releases/download/v5.0.0/TShock-5.0.0-for-Terraria-1.4.4.7-linux-arm-Release.zip --output tshock_release.zip
RUN unzip tshock_release.zip -d /tmp/tshock 
RUN cd /tmp/tshock 
RUN tar -xvf TShock-Beta-linux-arm-Release.tar 
RUN cd $(find . -name TShock.Server -printf '%h\n') 
RUN mv ./* /tshock/ 
RUN rm -R /tmp/* 
RUN chmod +x /tshock/TShock.Server

COPY run-tshock.sh /tshock/run.sh

# Commit Hash Metadata
ARG VCS_REF
LABEL org.label-schema.vcs-ref=$VCS_REF \
    org.label-schema.vcs-url="https://github.com/beardedio/terraria"

# Allow for external data
VOLUME ["/config", "/tshock/ServerPlugins"]

# Run the server
WORKDIR /tshock
ENTRYPOINT ["./run.sh"]
