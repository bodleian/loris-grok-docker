Docker build of Loris 2.0.1 IIIF Image Server with Grok 1.0 on Ubuntu 14.04
===========


A Dockfile deployment of Loris image server with Grok @ https://github.com/GrokImageCompression/grok and https://github.com/loris-imageserver/loris

Docker hub respository @ https://hub.docker.com/r/bdlss/loris-grok-docker/

Build successes are logged @ https://hub.docker.com/r/bdlss/loris-grok-docker/builds/

Dockerfile forked from https://github.com/loris-imageserver/loris-docker/blob/development/Dockerfile and changed to use Pillow/OPENJPEG 2.0.1.

IIIF validator v 1.0.0 @ https://pypi.python.org/pypi/iiif-validator/1.0.0

Please also refer to https://github.com/loris-imageserver/loris/issues/61 and https://github.com/ruven/iipsrv/pull/61#issuecomment-222381601

### Use  pre-built image
Download image from docker hub. Defaults to `latest` tag. Docker will normally run as root unless otherwise configured.

    $ docker pull bdlss/loris-grok-docker

To run the docker command without sudo, you need to add your user (who must have root privileges) to the docker group. To do this run following command:

	$ sudo usermod -aG docker <user_name>
	
### Build from scratch (optional)	
Use local Dockerfile to build image. Defaults to `latest` tag.

    $ sudo docker build -t bdlss/loris-grok-docker .

### Start the container

    $ docker run -d -p 5004:5004 bdlss/loris-grok-docker

### Images

Loris bundles with it's own test images. They are stored at `/usr/local/share/images/`.

### Test

Point your browser to `http://<Host or Container IP>:5004/01/02/0001.jp2/full/full/0/default.jpg`

e.g.

`http://localhost:5004/01/02/0001.jp2/full/full/0/default.jpg`

After starting the container, you can IIIF validate your images from the container command line:

To get to the container command line use:

```bash
docker ps
docker exec -it <container ID> /bin/bash
```

Then for an image served at `http://localhost:5004/prefix/image_id` the validator can be run with:

    $ python /tmp/iiif-validate.py -s localhost:5004 -p prefix -i image_id --version=2.0 -v

e.g.

    $ python /tmp/iiif-validate.py -s localhost:5004 -p '01/02' -i 0001.jp2 --version=2.0 -v

### Documentation and examples

Further documentation and examples are available here https://github.com/loris-imageserver/loris-docker
