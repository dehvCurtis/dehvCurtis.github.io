# Gauntlt Arachni Installation

[Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)

[Install Gruyere](https://github.com/dehvCurtis/Scripts/blob/master/DevSecOps/gruyere_installation.sh)

```bash
echo 'Pulling latest'
docker pull karthequian/gruyere:latest
echo 'Running Docker Containter on port 8008'
docker run --rm -d -p 8008:8008 karthequian/gruyere:latest
echo 'Listing running containers'
docker ps
```

Install Gauntlt
Clone `https://github.com/gauntlt/gauntlt-docker.git`

`cd gauntlt-docker`

[Replace Dockerfile with custom](https://github.com/dehvCurtis/Tools/blob/master/DevSecOps/Containers/Guantlt_custom/Dockerfile)

Options: `make help`

Log in to box:

`docker run --rm -it --entrypoint /bin/bash gauntlt`

Test