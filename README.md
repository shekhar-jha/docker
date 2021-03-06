Repository of docker build files

## jhash-base (jhash/oel-base)

This is the base image that gets the OEL (Oracle Enterprise Linux) 7 image ready for other installations. Refer to [jhash-base](https://github.com/shekhar-jha/docker/tree/master/jhash-base) for more details about image and tags available. Available through docker hub using command
```
docker pull jhash/oel-base
```

## jhash-control (jhash/control)

This is the control image that provides shared services like consul to rest of infrastructure. Refer to [jhash-control](https://github.com/shekhar-jha/docker/tree/master/jhash-control) for more details about image and tags available. Available through docker hub using command
```
docker pull jhash/control
```


## Additional information

1. Please refer to [Installation Process](https://github.com/shekhar-jha/scripts/blob/master/OEL_Installation.md) used to setup base OEL 7.1 server with docker.
