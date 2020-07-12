# DevSecOps 
## Secure CI/CD Pipeline

### AWS Cloud Specs

- 4 EC2 Ubuntu 18.04 Boxes
  - box1: Jenkins, Docker, Java, Maven
  - box2: Java, Tomcat
  - box3: Docker, JIRA
  - box4: Docker, DefectDojo
  
### Build Out
#### Box1
- [Install Jenkins](https://dehvcurtis.github.io/Wiki/Jenkins/installation)
- Install Jenkins Plugins
  - Maven Integration
  - BlueOcean Aggregator
  - Deploy To Container
  - SSH Agent
- Install Maven
  - `apt install maven`
  - `mvn -version`
  - Add maven install directory `/usr/share/maven/` to Jenkins Global Tool Configuration
    - Manage Jenkins -> Global Tool Configuration -> Maven -> Add Maven
 - [Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)

