Docker build of Loris 2.0.1 IIIF Image Server with OPENJPEG 2.1
===========


A Dockfile deployment of Loris image server with OPENJPEG @ https://github.com/uclouvain/openjpeg and https://github.com/loris-imageserver/loris

Docker hub respository @ https://hub.docker.com/r/bdlss/loris-openjpeg-docker/

Build successes are logged @ https://hub.docker.com/r/bdlss/loris-openjpeg-docker/builds/

Dockerfile forked from https://github.com/loris-imageserver/loris-docker/blob/development/Dockerfile and changed to use Pillow/OPENJPEG 2.0.1.

Please also refer to https://github.com/loris-imageserver/loris/issues/61 

### Use  pre-built image
Download image from docker hub. Defaults to `latest` tag. Docker will normally run as root unless otherwise configured.

    $ docker pull bdlss/loris-openjpeg-docker

To run the docker command without sudo, you need to add your user (who must have root privilages) to the docker group. For this run following command:

	$ sudo usermod -aG docker <user_name>
	
### Build from scratch
Use local Dockerfile to build image. Defaults to `latest` tag.

    $ sudo docker build -t bdlss/loris-openjpeg-docker .

### Start the container and test

    $ docker run -d -p 5004:5004 bdlss/loris.openjpeg

Point your browser to `http://<Host or Container IP>:5004/01/02/0001.jp2/full/full/0/default.jpg`

After starting the container, you can IIIF validate your images from the container command line:

To get to the container command line use:

```bash
docker ps
docker exec -it <container ID> /bin/bash
```

Then:

`/tmp/iiif-validator-0.9.1/iiif-validate.py -s localhost:80 -p "fcgi-bin/iipsrv.fcgi?IIIF=" -i var/www/localhost/images/67352ccc-d1b0-11e1-89ae-279075081939.jp2 --version=2.0 -v` 

### Documentation and examples

Further documentation and examples are available here https://github.com/loris-imageserver/loris-docker
