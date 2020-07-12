# Jenkins Installation

Installation Documentation: https://www.jenkins.io/doc/book/installing/

### Ubuntu 18.04
1. Install packages

`apt install openjdk-8-jdk`

`wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -`

`sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'`

`sudo apt-get update`

`sudo apt-get install jenkins`

2. Confirm port 8080 is open for inbound

3. Direct browser to http://`<ip-address>`:8080