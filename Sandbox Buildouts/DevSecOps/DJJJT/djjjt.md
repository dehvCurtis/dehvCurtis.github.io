# DevSecOps 
## Secure CI/CD Pipeline Buildout

### AWS Cloud Specs

- 4 EC2 Ubuntu 18.04 Boxes
  - box1: Jenkins, Docker, Java, Maven
  - box2: Java, Tomcat
  - box3: Docker, JIRA
  - box4: Docker, DefectDojo
 # 
### Server Build
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

#### Box2
- [Install Tomcat](https://dehvcurtis.github.io/Wiki/Tomcat/installation)

#### Box3
#### Box4
#
### Pipeline Build
#### On Box1
Create new pipeline item
  - [Clone webapp repo](https://github.com/dehvCurtis/WebApp_Samples)
  - Open Jenkins UI
  - Click `New Item` and name item
  - Select `Pipeline` option and click `ok`
  - Under `General` tab
    - Add description `DevSecOps pipeline demo`
    - Check `Discard old builds` box
      - `Max # of builds to keep`: 2
    - Check `GitHub project` box
      - Add [webapp repo](https://github.com/dehvCurtis/WebApp_Samples) url
    - Check `GitHub hook trigger for GITScm polling` box (monitors for new commits)
    - Check `Poll SCM Schedule` box
      - Add `* * * * *` (cron formatting)
    - Define pipeline
      - Definition: `Pipeline script from SCM`
      - SCM: `Git`
        - Repository URL: [webapp repo](https://github.com/dehvCurtis/WebApp_Samples)
        - Branches to build: `*/master` branch
    - Click `Save`