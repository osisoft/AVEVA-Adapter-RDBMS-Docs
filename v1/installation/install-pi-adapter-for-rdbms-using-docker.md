---
uid: InstallPIAdapterForRDBMSUsingDocker
---

# Install PI Adapter for RDBMS using Docker

Docker is a set of tools that can be used on Linux to manage application deployments. This topic provides examples of how to create a Docker container with the RDBMS adapter.

**Note:** If you want to use Docker, you must be familiar with the underlying technology and have determined that it is appropriate for your planned use of the RDBMS adapter. Docker is not a requirement to use the adapter.

## Create a startup script for the adapter

1. Use a text editor and create a script similar to one of the following examples:

	**Note:** The script varies slightly by processor.

	**ARM32**

	```bash
	#!/bin/sh
	if [ -z $portnum ] ; then
			exec /RDBMS_linux-arm/OSIsoft.Data.System.Host
	else
			exec /RDBMS_linux-arm/OSIsoft.Data.System.Host --port:$portnum
	fi
	```
	
	**ARM64**
	
	```bash
	#!/bin/sh
	if [ -z $portnum ] ; then
			exec /RDBMS_linux-arm64/OSIsoft.Data.System.Host
	else
			exec /RDBMS_linux-arm64/OSIsoft.Data.System.Host --port:$portnum
	fi
	```
	
	**AMD64**
	
	```bash
	#!/bin/sh
	if [ -z $portnum ] ; then
			exec /RDBMS_linux-x64/OSIsoft.Data.System.Host
	else
			exec /RDBMS_linux-x64/OSIsoft.Data.System.Host --port:$portnum
	fi
	```
	
2. Name the script `rdbmsdockerstart.sh` and save it to the directory where you plan to create the container.

## Create a Docker container containing the adapter

1. Create the following `Dockerfile` in the directory where you want to create and run the container. 

	**Note:** `Dockerfile` is the required name of the file. Use the variation according to your operating system.

	### ARM32

	```dockerfile
	FROM ubuntu
	WORKDIR /
	RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu60 libssl1.1 curl
	COPY rdbmsdockerstart.sh /
	RUN chmod +x /rdbmsdockerstart.sh
	ADD ./RDBMS_linux-arm.tar.gz .
	ENTRYPOINT ["/rdbmsdockerstart.sh"]
	```
	
	### ARM64

	```dockerfile
	FROM ubuntu
	WORKDIR /
	RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu66 libssl1.1 curl
	COPY rdbmsdockerstart.sh /
	RUN chmod +x /rdbmsdockerstart.sh
	ADD ./RDBMS_linux-arm64.tar.gz .
	ENTRYPOINT ["/rdbmsdockerstart.sh"]
	```

	### AMD64 (x64)

	```dockerfile
	FROM ubuntu
	WORKDIR /
	RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y ca-certificates libicu66 libssl1.1 curl
	COPY rdbmsdockerstart.sh /
	RUN chmod +x /rdbmsdockerstart.sh
	ADD ./RDBMS_linux-x64.tar.gz .
	ENTRYPOINT ["/rdbmsdockerstart.sh"]
	```

2. Copy the `RDBMS_linux-\<platform>.tar.gz` file to the same directory as the `Dockerfile`.

3. Copy the `rdbmsdockerstart.sh` script to the same directory as the `Dockerfile`.

4. Run the following command line in the same directory (you may need to use the `sudo` command):

	```bash
	docker build -t rdbmsadapter .
	```

## Run the adapter Docker container

### REST access from the local host to the Docker container

Complete the following steps to run the container:

1. Use the docker container image `rdbmsadapter` that you created previously.
2. Type the following command line (you may need to use the `sudo` command):

	```bash
	docker run -d --network host rdbmsadapter
	```

The default port `5590` is accessible from the host and you can make REST calls to RDBMS adapter from applications on the local host computer. In this example, all data stored by the adapter is stored in the container itself. When the container is deleted, the data stored is also deleted.

### Provide persistent storage for the Docker container

Complete the following to run the container:

1. Use the docker container image `rdbmsadapter` created previously.
2. Type the following command line (you may need to use the `sudo` command):

	```bash
	docker run -d --network host -v /rdbms:/usr/share/OSIsoft/ rdbmsadapter
	```

The default port `5590` is accessible from the host and you can make REST calls to the RDBMS adapter from applications on the local host computer. In this example, data is written to a host directory on the local machine `/rdbms` rather than the container. You can specify any directory.

### Port number change

To use a different port other than the default `5590`, you can specify a `portnum` variable on the `docker run` command line. For example, to start the RDBMS adapter using port `6000` instead of `5590`, use the following command line:

```bash
docker run -d -e portnum=6000 --network host rdbmsadapter
```

This command accesses the REST API with port `6000` instead of port `5590`. The following `curl` command returns the configuration for the container.

```bash
curl http://localhost:6000/api/v1/configuration
```

### Remove REST access to the Docker container

If you remove the `--network host` option from the docker run command, REST access is not possible from outside the container. This can be valuable when you want to host an application in the same container as the RDBMS adapter but do not want to have external REST access enabled.
