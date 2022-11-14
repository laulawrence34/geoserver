# A geoserver docker image

This Dockerfile will install the following dependencies:

* Debian based Linux
* OpenJDK 11
* Tomcat 9
* GeoServer
  * Support of custom fonts (e.g. for SLD styling)
  * CORS support
  * Support extensions
  * Support additional libraries

## How to Use

Run the command below to pull an official image: ``docker.osgeo.org/geoserver:{{VERSION}}``, e.g.:

```
docker pull docker.osgeo.org/geoserver:2.21.1
```

Then run the pulled image locally:

```
docker run -it -p 80:8080 docker.osgeo.org/geoserver:2.21.1
```

Check http://localhost/geoserver to see the geoserver page,
and login with geoserver default `admin:geoserver` credentials.

### How to build and deploy this image to a Digital Ocean enviornment

The following outlines the steps to create a CI/CD with Docker, GiHub Actions, & Digital Ocean (Droplet + Container Registry).
First you will need to clone this repository and then setup the following secrets in your Github repo:

**DIGITALOCEAN_ACCESS_TOKEN:** created in your Digital Ocean account<br />
**HOST:** the url of your Digital Ocean "droplet"<br />
**SSHKEY:** created using ssh-keygen on your "droplet"<br />
**USERNAME:** the username of your SSH user to access Digital Ocean server<br />
**PASSPHRASE:** the passphrase used for your SSH user<br />
**REGISTRY:** the url for your Digital Ocean "Container Registry" <br />
~~**~~UBUNTU_VERSION:~~** for the Ubuntu version that you want to use, eg. "ubuntu-latest" or "ubuntu-22.04"~~

~~Note: your **~~UBUNTU_VERSION~~** secret should correspond to the version number in the **UBUNTU_VERSION** variable in the Dockerfile as well.~~
Unfortunately, I could not get the Ubuntu version parameterized using a secret in the Github workflow, so if you need to change the Ubuntu version in the Github action file: deploy.yml you will need to explicitly specify the Ubuntu version, eg. "ubuntu-22.04" in the "runs-on" parameter for the "build_and_push" and the "deploy" job. You can however change the ubuntu version in the Dockerfile on line 1.

If you are using Ubuntu 22.04 version, there is an issue related to openssh 8.8p1-1 and up not accepting ssh-rsa key types by default and the workaround is to add "PubkeyAcceptedKeyTypes=+ssh-rsa" to your /etc/ssh/sshd_config file on the host server: https://github.com/appleboy/ssh-action/issues/157

### Custom configuration of Geoserver

For further configuration of your Geoserver setup, such as how to mount an external folder for use as a data 
or how to start a GeoServer without sample data, or how to download and install additional extensions on startup, or how to install additional extensions from local folder and other custom build parameters please refer to the original repository that this is based on:https://github.com/geoserver/docker 

### How to build a specific GeoServer version?

You can specify a Geoserver version to use by changing the GS_VERSION value in the Dockerfile. 

## Authors and acknowledgment
Special thanks to the contributors at https://github.com/geoserver/docker for creating this docker image
