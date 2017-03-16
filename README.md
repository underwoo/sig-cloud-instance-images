This branch is useful for creating a data1-like container

## Build a docker image

To build the docker, enter the `docker` directory and run the command:

```
docker build -t <image_name> .
```

where `<image_name>` is any string.  You will use this `<image_name>` later when 
launching the container.

Once the docker image is built, you can see the docker image by running:

```
docker image ls
```

You should see the `<image_name>` in the list of docker containers.

## Launch image

This docker image is configured to export two volumes:

* /var/www/cgi-bin 
* /var/www/html

These two volumes must reside on the host system.  The docker container should be able
to read these two volumes.  The data for these two volumes must be copied in from
the current data1.  (They are not included in the repository).  Changing permissions on 
the volumes may be reqruied for the apache version in the container to be able to read
the files.

The image is also configured to export two ports:

* 80
* 443

This is useful to view the pages from a web browser.

To launch the command, run the following command:

```
docker run -p <host_port_80>:80 -p <host_port_443>:443 -v <host_cgi_bin>:/var/www/cgi-bin -v <host_html>:/var/www/html --name <container_name> <image_name>
```

where `<host_port_80>` is the port on the host machine that will be mapped to the container's port 80, 
`<host_port_443>` is the port on the host machine that will be mapped to the container's port 443, 
`<host_cgi_bin>` is the path to the host's directory that contains the cgi-bin scripts from data1, 
`<host_html>` is the path to the host's directory that contains the html files from data1, 
`<container_name>` is a name give to the container, and `<image_name>` is the name given to the image
as before.

## Stopping the container

The container can be stopped by running:

```
docker stop <container_name>
```

It may require also hitting `Ctrl-C` serveral times.  I haven't figured out a clean way to get this 
container to both launch apache, and kill apache.

## Restart container

If you do stop the container, it can be relanuched by running:

```
docker start <container_name>
```

There will be no need to rebuild the image, or run the `docker run` command if the image has not changed.

## Running multiple instances on one host

It is possible to run multiple instances of the same container on a single host.  To do this, the cgi-bin
and html directories will need to be duplicated on the host machine, and two additional ports will need to
used as forwarding ports.  Once the new volume locations and ports are known, run a similar `docker run`
command as before:

```
docker run -p <host_port_80_new>:80 -p <host_port_443_new>:443 -v <host_cgi_bin_new>:/var/www/cgi-bin -v <host_html_new>:/var/www/html --name <container_name_new> <image_name>
```

where the `<*_new>` have the same definition as before, but point to the new volume and port values.  It is 
important to make sure to give the container a different name than the previous container.

