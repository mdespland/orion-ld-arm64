The main idea is to install a IoT data collector FIWARE based on a RPI4 with LoraWan concentrator

# Install the RPI4 with arm64 OS

## Clone this repo




# Install Orion-LD with Mintaka

## RUN Orion-LD + Mintaka

``` 
cd docker\orionld-mintaka
docker-compose up -d
```

## Rebuild your images

### Clone the repository

``` 
git clone https://github.com/mdespland/orion-ld-arm64.git
cd orion-ld-arm64
git submodule update --remote --recursive
``` 

### Orion-LD

The official [Orion-LD](https://github.com/FIWARE/context.Orion-LD) is not available for arm64 architecture. The build process need some adaptation in order to work on my RPI4.

I have fork the FIWARE_8.0 version [Orion-LD](https://github.com/mdespland/context.Orion-LD/tree/FIWARE_8.0_arm64) with some change in order to be able to build it. 
I have removed mongo installation from the orion-ld-base image (just keep the driver)

The build process is the same as the normal brocker

``` 
cd fiware/context.Orion-LD/docker
docker build -t marcdespland/orion-ld-base:1.0 -f Dockerfile-base ..
docker build -t marcdespland/orion-ld:FIWARE_8.0_arm64 -f Dockerfile ..
``` 

The result docker image is available on dockerhub : ```docker pull marcdespland/orion-ld:FIWARE_8.0_arm64``` 

### Mintaka

Mintaka is built on java, the standard docker image doesn't work on arm64, but it is easy to rebuild using the satndard process (with some change for the target source image not compatible with arm64)


``` 
cd fiware/mintaka/docker
cp ../../mintaka-docker/Dockerfile .
docker build -t marcdespland/mintaka -f Dockerfile ..
``` 
### timescaledb-postgis:latest-pg12

This version was not built for arm64, I made some litle changes on the Dockerfile in th [official repo](https://github.com/timescale/timescaledb-docker)

``` 
cd timescaledb-postgis
docker build --build-arg POSTGIS_VERSION=2.5.5 --build-arg PG_VERSION_TAG=pg12 -t marcdespland/timescaledb-postgis:latest-pg12 .
```