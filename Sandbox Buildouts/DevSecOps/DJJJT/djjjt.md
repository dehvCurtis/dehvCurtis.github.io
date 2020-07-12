# DevSecOps 
## Secure CI/CD Pipeline Buildout
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
    - Create `Jenkinsfile` file with no extension - [example](https://github.com/dehvCurtis/webapp_sample/blob/master/Jenkinsfile.stage1)
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
- Add SSH key to Box1 (Jenkins)
  - For this walkthrough, I'll be using the SSH key I use to connect my Mac to my AWS nodes. This is not a secure practice.
  - On Local Machine, get AWS SSH key
  - `cat ~/.ssh/<key>.pem`
  - Log on to Box1 (Jenkins)
  - `nano ~/.ssh/<key>.pem` use same key name as local machine
  - `chmod 400 ~/.ssh/<key>.pem`
  - While we are on this box, let's give webapp enough permissions. Make sure you are logged into Box2 as `ubuntu` user
  - `chmod 770 /opt/tomcat/webapps/`
- Open Jenkins UI
  - Click `Jenkins > Credentials > System > Global credentials (unrestricted) > Add Credentials`
    - Kind: `SSH Username with private key`
    - ID: `tomcat_server`
    - Description: `Tomcat Server`
    - Username: `ubuntu`
    - Private Key: `<paste private key from above>`

- Open `Jenkinsfile` in GitHub webapp repo
  - Add the following to the `Jenkinsfile` [example](https://github.com/dehvCurtis/webapp_sample/blob/master/Jenkinsfile.stage2). Don't forget to add your IP address. Commit so Jenkins will see the update
- Open Jenkins UI
  - Click `webapp-cicd-pipeline` pipeline
  - Click `Build Now`
  - Confirm build is successful
- Open browser
 - `http://<DNS or IP Adress>:8080/webapp/`
 - You should see a bootstrap homepage
