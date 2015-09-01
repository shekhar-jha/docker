This is the base image that gets the OEL (Oracle Enterprise Linux) 7 image ready for other installations. It

1. Updates and applies all the patches and installs basic tools like tar, unzip, wget, jq (javascript parser)
2. Downloads and install JDK in /opt/. It also exports JAVA_HOME=/opt/java environment variable 
3. Configure a new user "app" 
4. Exports volume /var/log/app & /opt/app
5. Install [s6](http://skarnet.org/software/s6/) for running multiple services.

# Tag
> latest - OEL (Oracle Enterprise Linux) 7.0, Oracle JDK (1.8.0_60)

# Build 
Build the image using command
```
docker build --rm=true --force-rm=true --pull=true -t jhash/oel-base .
```
# Run
Run a new docker image using command
```
docker run -i -t --rm jhash/oel-base bash
```
# Additional details

* Base Image Size (OEL 7.0) : 249M
* Final Size : 788M (178M from yum update + 353M from JDK + 6M from s6 & jq)
