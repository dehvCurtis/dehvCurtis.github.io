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
 - Install Jenkins http://ec2-54-153-95-229.us-west-1.compute.amazonaws.com/Jenkins/Install
 - Install Jenkins Plugins
  * Maven Integration
	* Blue Ocean
	* Deploy To Container
	* SSH Agent
 - Install Maven
 `apt install maven`
 `mvn -version`

 - Install Docker http://ec2-54-153-95-229.us-west-1.compute.amazonaws.com/e/en/Docker/Install

