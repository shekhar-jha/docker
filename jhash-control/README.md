This docker build script creates a container with components that part of the control infrastructure. It consists of
1. Consul 
2. Zookeeper

This is built over [jhash/oel-base](https://github.com/shekhar-jha/docker/tree/master/jhash-base) which is an extension of Oracle Enterprise Linux 7.0.

All the services are configured and managed through the [s6](http://skarnet.org/software/s6/) init system (part of oel-base).

# Tag
## latest 
1. OEL (Oracle Enterprise Linux) 7.0 (oel-base)
2. Oracle JDK (1.8.0_60) (oel-base)
3. jq (1.5) (oel-base)
4. s6-overlay (1.11.0.1) (oel-base)
5. Consul (0.5.2)
6. Consul Template (0.10.0)
7. Zookeeper (3.4.6)

# Build
Build the image using command
```
docker build --rm=true --force-rm=true --pull=true -t jhash/control .
```

# Run

The basic run command would be
```
docker run -i -t --rm jhash/control
```

## Consul

By default consul attaches it's HTTP API, DNS interfaces to 127.0.0.1 IP Address. This is changed to attach the interfaces to IPv4 address assigned to server.


The following environment variables can be used to control consul service

### CONSUL_OPTIONS
All the values passed through CONSUL_OPTIONS will be passed on command line

The following script starts two consul servers with bootstrap expectation of 2 in micro1 datacenter

```
CID=$(docker run -d -e "CONSUL_OPTIONS=-bootstrap-expect 2 -dc micro1" jhash/control);
echo "Started container ${CID}"
IPADDRESS=`docker inspect ${CID}|grep "\"IPAddress" | cut -d : -f 2 | cut -d \" -f 2`
echo "Starting container and joining consul to ${IPADDRESS}"
docker run -i -t --rm  -e "CONSUL_OPTIONS=-bootstrap-expect 2 -dc micro1 -retry-join=${IPADDRESS}" jhash/control
```

### CONSUL_SERVER
This parameter can be IP Address or name of consul server that will be used to lookup all the consul service providers. The new server will try to join all the servers thus identified.

The following script will start a new server with consul service for bootstrapping. It then starts another consul service that joins the first consul server's.

```
CID=$(docker run -d -e "CONSUL_OPTIONS=-bootstrap-expect 1" jhash/control);
echo "Started container ${CID}"
IPADDRESS=`docker inspect ${CID}|grep "\"IPAddress" | cut -d : -f 2 | cut -d \" -f 2`
echo "Starting container and joining consul to ${IPADDRESS}"
docker run -i -t --rm  -e "CONSUL_SERVER=${IPADDRESS}" jhash/control
```
### CONSUL_DISABLE
This parameter if set (any value) will disable the consul service. This will ensure that consul will not be started when the container is started.

### HOST_IP
if HOST_IP is set with an IP Address, the consul will set the advertise (-advertise option) IP address to the given IP address.

# Zookeeper

The following environment variables can be used to control zookeeper service

### CONSUL_SERVER
The consul server if specified will be used to 
1. deregister the service when the s6 is shutdown as part of server shutdown or due to ctrl+c on console
2. 

### ZOOKEEPER_DISABLE
This parameter if set (any value) will disable the zookeeper service. This will ensure that zookeeper will not be started when the container is started.

### ZK_ID
Defines the zookeeper ID associated with the server. If not specified, the startup process tries to identify the ID using the zookeeper servers registered with consul. The default value if nothing else can be set is 1

### HOST_IP
If HOST_IP is set with an IP Address, the consul template will register HOST_IP IP address with consul instead of container IP address.

# Additional Details

## Size
|Image | Size (df -kh / & du -sh /opt/app) | Size (docker images) |
|------|-----------------------------------|----------------------|
|oraclelinux:7.0 |  | 249M |
| oel-base       | 788M | 892M |
| control        | 856M + 59M | 963.2M |

