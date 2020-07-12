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
 - [Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)
#### Box4
 - [Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)
### Pipeline Build
#### On Box1
Create new pipeline item
  - [Clone webapp repo](https://github.com/dehvCurtis/webapp_sample.git)
  - Open Jenkins UI
      - Click `New Item`
      - Name item `webapp-cicd-pipeline`
      - Select `Pipeline` option and click `ok`
      - Under `General` tab
        - Add description `DevSecOps pipeline demo`
        - Check `Discard old builds` box
          - `Max # of builds to keep`: 2
        - Check `GitHub project` box
          - Add [webapp repo](https://github.com/dehvCurtis/webapp_sample.git) url
        - Check `GitHub hook trigger for GITScm polling` box (monitors for new commits)
        - Check `Poll SCM Schedule` box
          - Add `* * * * *` (cron formatting)
        - Define pipeline
          - Definition: `Pipeline script from SCM`
          - SCM: `Git`
            - Repository URL: [webapp repo](https://github.com/dehvCurtis/webapp_sample.git)
            - Branches to build: `*/master` branch
        - Click `Save`
        - Go to Jenkins home in dashboard
          - Click `Manage Jenkins` > `Global Tool Configuration`
          - Under `Git` section, Check the `Install automatically` box
  - Open `webapp` Git Repo
    - Create `Jenkinsfile` file - [example](https://github.com/dehvCurtis/webapp_sample/blob/master/Jenkinsfile)
    - Commit changes
  - Open Jenkins UI
    - Click on `webapp-cicd-pipeline` pipeline
    - Click `Build Now` on left
    - Confirm build is successful
    - Click `Open Blue Ocean` on left
    - Confirm successful build
#    
## Automating Deployment to Tomcat in Pipeline
#### Deploy to Prod (`/opt/tomcat`)
Add SSH key for Jenkins -> Tomcat
- SSH into Box2 (Tomcat Server)
- [Create SSH key](https://dehvcurtis.github.io/Wiki/Linux/SSH/)