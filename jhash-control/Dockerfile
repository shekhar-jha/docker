# The docker image with shared infrastructure
# 1. Consul    (Service Registeration and Discovery AND DNS)
# 2. Zookeeper (dependency for storm)
# 3. ??        (Log collector)
# 4. ??        (Performance collector)
# 5. ??        (??)
FROM jhash/oel-base

####   Consul   ####
ENV APP_BASE=/opt/app APP_NAME=consul CONSUL_VERSION=0.5.2
ENV CONSUL_URL=https://dl.bintray.com/mitchellh/consul/${CONSUL_VERSION}_linux_amd64.zip CONSUL_OPTIONS=""
WORKDIR /tmp/
ADD $CONSUL_URL /tmp/
ADD consul/consul_run $APP_BASE/services.d/$APP_NAME/run
ADD consul/01-consul-init /etc/cont-init.d/01-consul-init
RUN unzip /tmp/${CONSUL_VERSION}_linux_amd64.zip && \
    chmod +x /tmp/consul && \
    rm -f /tmp/${CONSUL_VERSION}_linux_amd64.zip
EXPOSE 8300 8301 8302 8400 8500 8600


WORKDIR /opt/app
ENTRYPOINT ["/init"]
