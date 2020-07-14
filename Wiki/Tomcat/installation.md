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
  
- Create a systemd Service File
  - `update-java-alternatives -l`
  - `sudo nano /etc/systemd/system/tomcat.service`
  - Add the following

```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

Run
  
`systemctl daemon-reload && systemctl start tomcat`

- Open Port
  - port 8080
  - confirm working via http://`<ip address>`:8080