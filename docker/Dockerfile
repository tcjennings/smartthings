FROM tcjennings/rpi-node:lts-armv6

ARG architecture="armv6"
ARG build_date
ARG repo="redloro"
ARG branch="master"

LABEL org.label-schema.schema-version="1.0" \
      org.label-schema.name="rpi-smartthings-nodeproxy" \
      org.label-schema.description="SmartThings Node Proxy for Raspberry Pi" \
      org.label-schema.version="1.0.2" \
      org.label-schema.docker.cmd="docker run -d -p 8080:8080 -e ENABLED_PLUGINS='' --device=/dev/ttyUSB0 rpi-smartthings-nodeproxy" 

RUN apk add --virtual runtime-dependencies python udev libpcap \
 && apk add --virtual build-dependencies tar make g++ git linux-headers libpcap-dev \
 && mkdir -p /stnp/plugins \
 && wget -O - https://github.com/${repo}/smartthings/tarball/${branch} \
  | tar -xzvf - --wildcards --strip-components=2 -C /stnp/ ${repo}-smartthings-*/smartthings-nodeproxy/ \
 && cd /stnp \
 && rm -f restart.me smartthings-nodeproxy.service config.json \
 && npm install \
 && npm install serialport@^5.0.0 --build-from-source --unsafe-perm \
 && npm install cap --build-from-source \
 && apk del build-dependencies \
 && npm cache clean --force \
 && rm -rf /root/.node-gyp \
 && rm -rf /stnp/node_modules/serialport/build \
 && rm -rf /stnp/node_modules/cap/build \
 && rm -rf /var/cache/apk/*

COPY docker-entrypoint.sh /docker-entrypoint.sh
COPY config.json /stnp/config.json

EXPOSE 8080

ENTRYPOINT ["/docker-entrypoint.sh"]

