# Tomcat Installation
## Ubuntu 18.04

- Update apt package index
  - `sudo apt update`

- Install JDK package
  - `sudo apt install default-jdk`
  
- Create tomcat user
  - `sudo groupadd tomcat`
  - `sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat`

- Install Tomcat
  - `cd /tmp`
  - `curl -O http://www.gtlib.gatech.edu/pub/apache/tomcat/tomcat-9/v9.0.37/bin/apache-tomcat-9.0.37.tar.gz`
  - `mkdir /opt/tomcat`
  - `tar xzvf apache-tomcat-*tar.gz -C /opt/tomcat --strip-components=1`

- Update Permissions
  - `cd /opt/tomcat`
  - `sudo chgrp -R tomcat /opt/tomcat`
  - `sudo chmod -R g+r conf`
  - `sudo chmod g+x conf`
  - `sudo chown -R tomcat webapps/ work/ temp/ logs/`
  
