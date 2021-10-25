# Weekly assesment. Week 2
## Task 1. Create Jenkins VM with internet access(1 point)
### Installing openjdk-8-jdk and git: 
 ````sh
 $ sudo apt-get install openjdk-8-jre git
 ````
### Install Jenkins with enabling autostart on startup:
````sh
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add - 
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install -y jenkins
sudo systemctl enable jenkins
sudo systemctl jenkins status
````
### Setup custom port 8081 for Jenkins:
To setup custom port we need to correct the patametr HTTP_PORT from 8080 to 8081 in file /etc/default/jenkins:
<img width="594" alt="jenkins port" src="https://user-images.githubusercontent.com/84975945/137627682-ddda3c67-b3c2-4fab-93ed-5ae8460af4cf.png">
### Plugins – select plugins, add GitHub and Role-based authorization strategy:
<img width="1439" alt="Git plugin" src="https://user-images.githubusercontent.com/84975945/137627714-34528298-f88e-4afb-935d-59c5ca03d8d5.png">
<img width="1439" alt="Role-based Authorization Strategy" src="https://user-images.githubusercontent.com/84975945/137627720-61dd310f-114e-40f9-bd22-91f0e03ca2ae.png">
### Add new user – jenkins-NAME (your fullname, jenkins-linustorvalds):
<img width="1439" alt="jenkins user" src="https://user-images.githubusercontent.com/84975945/137627724-85c501d2-f85a-49a7-a6f7-7fab8a3c945d.png">
## Task 2. Create agent VM(1 point)
### Installing openjdk-8-jdk and git: 
 ````sh
 $ sudo apt-get install openjdk-8-jre git
````
### Prepare SSH keys: 
On master node we generete ssh keys(if we have no any keys yet). Than we add key /home/user/.ssh/id_rsa.pub from master node to /home/user/.ssh/authorized_keys
````sh
ssh-keygen #on master node to generate keys
cat .ssh/id_rsa.pub #on master node to get value of key
nano .ssh/authorized_keys #on slave node and paste the value of key from master here, than save the file
````
Than we can check ssh connection from master to slave via terminal, if it's successful we can add agent node to master in Jenkins.
### Connect agent node to master: 
To connect agent node we go to Manage Jenkins -> Manage nodes and clouds -> New node. Give the name to agent(this name we will use as label of agent)
<img width="1788" alt="adding node" src="https://user-images.githubusercontent.com/84975945/137627762-fe76b5b1-203d-476b-8bdf-99a87b96530d.png">
Than configure connection:
<img width="1566" alt="agent connection" src="https://user-images.githubusercontent.com/84975945/137627768-105c6600-c466-406d-a715-6e18e3730fca.png">
<img width="1351" alt="connection setting" src="https://user-images.githubusercontent.com/84975945/137627777-3b531660-a5c1-4b63-b076-2b6445660e1b.png">
<img width="1790" alt="agent online" src="https://user-images.githubusercontent.com/84975945/137627791-2c3e84e0-7b11-457e-a83e-0bbfeb8f60b1.png">

## Task 3. Configure tools – NodeJS(1 point):
<img width="1468" alt="tool conf" src="https://user-images.githubusercontent.com/84975945/137627810-26305b8e-c9e7-49f2-a796-dcabab1ff397.png">

## Task 4. Create “Multibranch Pipeline” pipeline job (work inside Lab folder)(3 points):
### Creating multibranch pipeline:
New item -> Multibranch Pipeline. Here we name our job and go to configuring it. </p>
We need to configure "Branch sources" and "Build configuration". Other configuration is optional. 
<img width="1250" alt="multibranch conf" src="https://user-images.githubusercontent.com/84975945/137627818-e781df2b-0701-44f7-8fb4-e4723395bc39.png">
<img width="1250" alt="multibranch conf1" src="https://user-images.githubusercontent.com/84975945/137627824-5a784689-2d6c-44d0-a2f2-c1e218bfff2f.png">
### Write Jenkinsfile which describes declarative pipeline:
#### a) Define NPM tools in pipeline section
#### b) Run in parallel stages for compressing JS, CSS files by using next utils:
- Uglify-js</p>
- cleans-css </p>
www/css -> www/min </p>
www/js -> www/min </p>
#### c) Create tar archive (ignore .git, css and js folders)
#### d) Archive result 
Declarative pipeline: https://github.com/Timur-Rustamov/material-design-template/blob/main/TimurRustamov/Jenkinsfile 
Scan log:
<img width="769" alt="scan log" src="https://user-images.githubusercontent.com/84975945/137627837-954eebbd-6ae5-4d88-83b0-c9889c78d952.png">
Build log - https://github.com/Timur-Rustamov/material-design-template/blob/main/Week2_CI_CD_tools/build%20log.txt
<img width="1093" alt="blue ocean" src="https://user-images.githubusercontent.com/84975945/137627855-1551b363-7370-47fe-8235-eb077005f503.png">
<img width="1117" alt="stage view" src="https://user-images.githubusercontent.com/84975945/137627860-4ad9b7a0-ad46-482b-829e-448047b40698.png">
## Task 5. Setup the GitHub webhook to trigger the jobs(2 points)
### Poll SCM in job settings:
To have controll of changes in repo i setup "Scan repository triggers" for scanning every 2 hours from the last scan.
<img width="1152" alt="scan repo" src="https://user-images.githubusercontent.com/84975945/137627868-63b3f52a-b5ee-4cec-9308-524153c68d28.png">
### GitHub hook:
To configure GitHub hook we go to repo settings -> Webhooks-> Add webhook: 
<img width="943" alt="web hook" src="https://user-images.githubusercontent.com/84975945/137627869-5aa16228-6e59-45ae-9aee-b3b25148faab.png">
<img width="1517" alt="web hook1" src="https://user-images.githubusercontent.com/84975945/137627872-4aca7ed2-d319-40fe-965c-acd4d4474d9e.png">
## Spin up VM with installed Artifactory
To install artifactory to VM we can use following commands:
 ````sh
 $ wget -qO - https://api.bintray.com/orgs/jfrog/keys/gpg/public.key | sudo apt-key add -
 $ echo "deb https://jfrog.bintray.com/artifactory-debs bionic main" | sudo tee /etc/apt/sources.list.d/jfrog.list
 $ sudo apt update
 $ sudo apt install jfrog-artifactory-oss
 $ sudo systemctl start artifactory.service
````
![image](https://user-images.githubusercontent.com/84975945/137643549-5a893e3a-b453-4ba0-b60a-eb231eb940c0.png)
## Pushing build to artifactory
Stage of pushing to artifactory is placed in main jenkinsfile - https://github.com/Timur-Rustamov/material-design-template/blob/main/TimurRustamov/Jenkinsfile
### Results of full pipeline
![image](https://user-images.githubusercontent.com/84975945/137733080-069cbe7c-6c2f-4275-9804-bd0c1344374d.png)
![image](https://user-images.githubusercontent.com/84975945/137733199-c1d4c9db-79de-4a64-9b24-b58977b21bd0.png)
## Use Scripted pipeline instead of declarative
Link to scripted pipeline - https://github.com/Timur-Rustamov/material-design-template/blob/main/Scripted%20Jenkinsfile
