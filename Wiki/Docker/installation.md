# Docker Installation

## Ubuntu 18.04

### Installation Walkthrough
(installation script found in section after walkthrough)

The Docker installation package available in the official Ubuntu repository may not be the latest version. To ensure we get the latest version, install Docker from the official Docker repository by performing the following

Update your existing list of packages

`sudo apt update`

Install prerequisite packages for package use over HTTPS

`sudo apt install apt-transport-https ca-certificates curl software-properties-common`

Add the GPG key for the official Docker repository to your system

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

Add the Docker repository to APT sources

`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`

Update the package database with the Docker packages from the newly added repo

`sudo apt update`

Ensure you are about to install from the Docker repo

`apt-cache policy docker-ce`

example output:

```
docker-ce:
  Installed: (none)
  Candidate: 18.03.1~ce~3-0~ubuntu
  Version table:
     18.03.1~ce~3-0~ubuntu 500
        500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
```

Install Docker

`sudo apt install docker-ce`

Check status

`systemctl status docker`

### Executing the Docker Command Without Sudo

Add to docker group

Run commands as `ubuntu` user:

`sudo usermod -aG docker $USER`

Log off `ubuntu` user and log back in to `ubuntu` user 

Check groups with:

`id -nG`

### Ubuntu 18.04 Installation Script (run w/ sudo)

```
apt update
apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
apt update
apt install docker-ce
echo 'Do not forget to check status of docker'
```