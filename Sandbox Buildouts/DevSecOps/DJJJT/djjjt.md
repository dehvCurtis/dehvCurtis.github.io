# DevSecOps 
## Secure CI/CD Pipeline Build-out
#### DJJJT = Docker, Java, Jenkins, JIRA, Tomcat
#
### AWS Cloud Specs

- 4 EC2 Ubuntu 18.04 Boxes
  - box1: Docker, Jenkins, Java, Maven
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
    - Remove `Install Automatically` check box
    - Name: `Maven`
    - MAVEN_HOME: `/usr/share/maven`
 - [Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)

#### Box2
- [Install Tomcat](https://dehvcurtis.github.io/Wiki/Tomcat/installation)

#### Box3
 - [Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)
#### Box4
 - [Install Docker](https://dehvcurtis.github.io/Wiki/Docker/installation)
 
### Pipeline Build
Create new pipeline item

SSH into Box1
  - [Clone webapp repo](https://github.com/dehvCurtis/WebApp.git) locally and create your own WebApp repo
  - Open Jenkins UI
      - Click `New Item`
      - Name item `webapp-cicd-pipeline`
      - Select `Pipeline` option and click `ok`
      - Under `General` tab
        - Add description `DevSecOps pipeline demo`
        - Check `Discard old builds` box
          - `Max # of builds to keep`: 2
        - Check `GitHub project` box
          - Add your webapp Git repo url
        - Check `GitHub hook trigger for GITScm polling` box (monitors for new commits)
        - Check `Poll SCM Schedule` box
          - Add `* * * * *` (cron formatting)
        - Define pipeline
          - Definition: `Pipeline script from SCM`
          - SCM: `Git`
            - Git WebApp Repository URL
            - Branches to build: `*/master` branch
        - Click `Save`
        - Go to Jenkins home in dashboard
          - Click `Manage Jenkins` > `Global Tool Configuration`
          - Under `Git` section, Check the `Install automatically` box
  - Open `webapp` Git Repo
    - Create `Jenkinsfile` file with no extension - [example](https://github.com/dehvCurtis/WebApp/blob/master/Jenkinsfile.stage1)
    - Commit changes
  - Open Jenkins UI
    - Click on `webapp-cicd-pipeline` pipeline
    - Click `Build Now` on left
    - Confirm build is successful
    - Click `Open Blue Ocean` on left
    - Confirm successful build

### Automating Deployment to Tomcat in CI/CD Pipeline - Deploying webapp to Prod (`/opt/tomcat`)

Add SSH key for Jenkins -> Tomcat
SSH into Box2 (Tomcat Server)
- Add SSH key to Box1 (Jenkins)
  - For this walkthrough, I'll be using the SSH key I use to connect my Mac to my AWS nodes. This is not a secure practice.
  - On Local Machine, get AWS SSH key
  - `cat ~/.ssh/<key>.pem`
  - Log on to Box1 (Jenkins)
  - `nano ~/.ssh/<key>.pem` use same key name as local machine
  - `chmod 400 ~/.ssh/<key>.pem`
  - Let's give webapp enough permissions. Make sure you are logged into Box2 as `ubuntu` user
  - `chmod 770 /opt/tomcat/webapps/`
- Open Jenkins UI
  - Click `Jenkins > Credentials > System > Global credentials (unrestricted) > Add Credentials`
    - Kind: `SSH Username with private key`
    - ID: `tomcat_server`
    - Description: `Tomcat Server`
    - Username: `ubuntu`
    - Private Key: `<paste private key from above>`

- Open `Jenkinsfile` in GitHub webapp repo
  - Add the following to the `Jenkinsfile` [example](https://github.com/dehvCurtis/WebApp/blob/master/Jenkinsfile.stage2)
- Open Jenkins UI
  - Click `webapp-cicd-pipeline` pipeline
  - Click `Build Now`
  - Confirm build is successful
- Open browser
  - `http://<DNS or IP Adress>:8080/webapp/`
  - You should see a bootstrap homepage

### Integrate Security Into CI/CD
#### Set up Trufflehog
- SSH into Box1 (Jenkins) to manually test docker job before adding to pipeline
  - `docker pull gesellix/trufflehog` https://hub.docker.com/r/gesellix/trufflehog
  - `docker images` to confirm container image
  - `docker run -t gesellix/trufflehog http://<github webapp repo url>.git > /tmp/truffle_output.json`
  - [sample output](https://github.com/dehvCurtis/WebApp/blob/master/truffle_output.json)
  - be sure `ubuntu` user is part of docker group
  - confirm successful
- Open `Jenkinsfile` in GitHub webapp repo
  - Add the following to the `Jenkinsfile` [example](https://github.com/dehvCurtis/WebApp/blob/master/Jenkinsfile.stage3)
  - Be sure to add security check before the build process, since we are looking for hidden secrets
- Open Jenkins UI
  - Click `webapp-cicd-pipeline` pipeline
  - Click `Build Now`
  - Click dot to check output
    - if permissions error similar to 
    
    `docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create: dial unix /var/run/docker.sock: connect: permission denied.`
    - try adding `jenkins` user to `docker` group
    `sudo usermod -a -G docker jenkins`
    - try and run `sudo chmod 666 /var/run/docker.sock` !insecure
    - rerun Jenkins build
    
#### Set up OWASP Dependency Check for Source Composition Analysis

OWASP Dependency Check Setup
- Create owasp file in webapp folder 
    
    `nano <absolute path>/owasp-dependency-check.sh`
- Copy latest script from https://hub.docker.com/r/owasp/dependency-check/ to `owasp-dependency-check.sh`
- Create test file on Box1 (Jenkins)
- Run script manually to download all dependencies (Jenkins tends to timeout)
  - `wget htttp://https://raw.githubusercontent.com/dehvCurtis/WebApp/master/owasp-dependency-check.sh`
  - `chmod +x owasp-dependency-check.sh`
  - `./owasp-dependency-check.sh`
- Open `Jenkinsfile` in GitHub webapp repo
  - Add the following to the `Jenkinsfile` [example](https://github.com/dehvCurtis/WebApp/blob/master/Jenkinsfile.stage4)
- Open Jenkins UI
  - Click `webapp-cicd-pipeline` pipeline
  - Click `Build Now`
  - Click dot to check output  