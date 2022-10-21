FROM ghcr.io/disic/designgouv-confiture/nginx:1.22.1-r0
MAINTAINER Olivier JAVAUX <olivier.javaux@ovhcloud.com>

ARG VERSION=1.0

# nginx config
ADD config /tmp/config/
ADD run_nginx /tmp/

# webapp files
ADD webapp /data/confiture/

# copy files
COPY run_nginx /tmp/

RUN \
  \
  # version to avoid cache \
  echo "version $VERSION" && \
  \
  # nginx config \
  rm -rf /etc/nginx && \
  mkdir /etc/nginx /etc/nginx_user && \
  tar -C /tmp/config/config_nginx -cf - . | tar -C /etc/nginx -xf - && \
  tar -C /tmp/config/config_nginx_user -cf - . | tar -C /etc/nginx_user -xf - && \
  rm -rf tmp/config && \
  \
  # start script \
  mv /tmp/run_nginx /root/ && \
  chmod u+x,go= /root/run_nginx && \
  \
  # set correct permissions \
  chown -R root:root /etc/nginx && \
  chown -R root:dinum_nginx /etc/nginx_user /data && \
  chmod -R go= /etc/nginx && \
  chmod -R g=rX,o= /etc/nginx_user /data && \
  \
  # cleanup \
  find /var/cache/apk /tmp /var/tmp /run /var/log -mindepth 1 -delete -print

# Entrypoint is tini, overwrite default container command to use ours
CMD ["/root/run_nginx"]

