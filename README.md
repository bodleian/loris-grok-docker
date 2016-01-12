Docker build of Loris IIIF Image Server with OPENJPEG
===========

Docker container running [Loris IIIF Image Server](https://github.com/loris-imageserver/loris)

Forked from https://github.com/loris-imageserver/loris-docker/blob/development/Dockerfile to use OPENJPEG 2.0.1.

OPENJPEG/Pillow install for JPEG2000 support obtained from http://shortrecipes.blogspot.co.uk/2014/06/python-34-and-pillow-24-with-jpeg2000.html.

### Use  pre-built image
Download image from docker hub.

    $ docker pull bdlss/loris.openjpeg

### Build from scratch
Use local Dockerfile to build image.

    $ docker build -t your_image_name .

### Start the container and test

    $ docker run -d -p 5004:5004 bdlss/loris.openjpeg

Point your browser to `http://<Host or Container IP>:5004/01/02/0001.jp2/full/full/0/default.jpg`

### Use samba to load images
Add the images directory as a volume and mount on a Samba or sshd container. [(See svendowideit/samba)](https://registry.hub.docker.com/u/svendowideit/samba/)

    $ docker run --name loris -v /usr/local/share/images -d -p 3000:3000 bdlss/loris.openjpeg
    $ docker run --rm -v /usr/local/bin/docker:/docker -v /var/run/docker.sock:/docker.sock svendowideit/samba loris
    

### Create loris cluster
Create data volume container

    $ docker run --name loris_data -v /usr/local/share/images -v /var/cache/loris -d ubuntu echo Data only container for loris images and cache

Create two loris server containers with shared image and cache volumes    

    $ docker run --name loris_server_1 --volumes-from loris_data -d bdlss/loris.openjpeg
    $ docker run --name loris_server_2 --volumes-from loris_data -d bdlss/loris.openjpeg
    
Build nginx image with custom config

    $ cd nginx
    $ docker build -t bdlss/loris.openjpeg .

Run nginx proxy

    $ docker run --name loris_proxy  --link loris_server_1:server1 --link loris_server_2:server2 -d -p 80:80 bdlss/loris.openjpeg
