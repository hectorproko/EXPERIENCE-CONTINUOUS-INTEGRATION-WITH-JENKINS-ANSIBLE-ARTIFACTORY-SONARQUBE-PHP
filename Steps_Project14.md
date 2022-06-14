# EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP
PROJECT 14


### ANSIBLE ROLES FOR CI ENVIRONMENT

Installing roles

Sonarqube
From <https://galaxy.ansible.com/lrk/sonarqube> 
artifactory
From <https://galaxy.ansible.com/bbaassssiiee/artifactory> 

``` bash
ubuntu@ip-172-31-94-159:~$ sudo ansible-galaxy install lrk.sonarqube
- downloading role 'sonarqube', owned by lrk
- downloading role from https://github.com/lrk/ansible-role-sonarqube/archive/v1.1.0.tar.gz
- extracting lrk.sonarqube to /home/ubuntu/ansible-config-artifact/roles/lrk.sonarqube
- lrk.sonarqube (v1.1.0) was installed successfully

ubuntu@ip-172-31-94-159:~$ sudo ansible-galaxy install bbaassssiiee.artifactory
- downloading role 'artifactory', owned by bbaassssiiee
- downloading role from https://github.com/bbaassssiiee/artifactory/archive/v1.1.2.tar.gz
- extracting bbaassssiiee.artifactory to /home/ubuntu/ansible-config-artifact/roles/bbaassssiiee.artifactory
- bbaassssiiee.artifactory (v1.1.2) was installed successfully
- adding dependency: dockpack.base_java8
- downloading role 'base_java8', owned by dockpack
- downloading role from https://github.com/dockpack/base_java8/archive/2.0.2.tar.gz
- extracting dockpack.base_java8 to /home/ubuntu/ansible-config-artifact/roles/dockpack.base_java8
- dockpack.base_java8 (2.0.2) was installed successfully
ubuntu@ip-172-31-94-159:~$
```

``` bash
hector@hector-Laptop:~/ansible-config-mgt/roles$ ls
apache  apacheBANK  artifactory  common  dockpack.base_java8  mysql  nginx  nginxBAK  sonarqube  webserver
```

Did this in the Laptop so now we push to get picked by Jenkins and copy to Ansible-Jenkins instance

``` bash
hector@hector-Laptop:~/ansible-config-mgt/roles$ git add artifactory/ sonarqube/ dockpack.base_java8/
hector@hector-Laptop:~/ansible-config-mgt/roles$ git commit -m "roles: sonarqube and artifactory+java dependency"
```

In Ansible-Jenkins instnace

``` bash
ubuntu@ip-172-31-94-159:~/ansible-config-artifact/roles$ ls
apache  artifactory  common  dockpack.base_java8  mysql  nginx  webservers
```


Install & Open Blue Ocean Jenkins Plugin  
Manage Jenkins > Plugin Manager  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/blueOcean.png)  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/dashboard.png)  


Creating Access tokein in Github  
Settings > Developer settings > Personal access tokens , button Generate new token  
Under Note "jenkins-access-token"read:user  
Select Scopes we put repo and on user select user:email user, read:user   




![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/token.png)  

`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`  



![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/storeCode.png)  


Created repo 
https://github.com/hectorproko/ansible-project  

At this point it tries to find a Jenkinsfile does not find it and prompts you for  a page to create  

This is where we ignore and click Administration  

Now we go back to Dashboard we the job (newly create pipeline)  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/pipeline.png)  

Created the Jenkinsfile with echo "Building Stage"  


Our ansible-project job has more options   

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/ansibleProject.png)  


We are going to go to Configure  

Build Configuration tab  
We are going to specify the path of the Jenkinsfile  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/buildConfig.png)  


Click Save  

Takes us back to ansible-project we get to pick branches we pick the only only one main  

Click Build Now to test  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/branchMain.png)  

If we check the Console Ouput of this build we see the intended message  
``` bash
	...
	...
	[Pipeline] sh
	+ echo Building Stage
	Building Stage
	[Pipeline] }
	...
	...
```


So we just triggered this job from regular Jenkins we could do it from  Open Blue Ocean GUI  
The logs from Ocean Blue is also really good  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/build.png)  


To appreciate the multibranch options we create another branch Create a new git branch and name it feature/jenkinspipeline-stages  

Added test stage to this branch  

For the new branch to show up Navigate to the ansible-project dashboard and click on "Scan repository now"  




![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/scanRepo.png)  


We get an nice interface  



![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/niceInterface.png)


Created more stages  

Test the branh with new stages in Blue Ocean branch feature/jenkinspipeline-stages  

Creating a pull request to merge it to main  

Test the job in Ocean Blue using main  



![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/ansibleProject8.png)


### RUNNING ANSIBLE PLAYBOOK FROM JENKINS
Install ansible plug-in


Screen clipping taken: 4/22/2022 3:07 PM
ansiblePlugin

Manage Jenkins >> Global Tool Configuration



Screen clipping taken: 4/22/2022 3:14 PM
addAnsible

To 
ubuntu@ip-172-31-94-159:~$ which ansible
/usr/bin/ansible



Screen clipping taken: 4/22/2022 3:18 PM
addAnsible2



Created job ansible-demo of type Pipeline


Now we type script directly to pipeline
You can use the Pipeline Syntax thing
	In Pipeline Syntax I looked for Git and I get to specify


	Screen clipping taken: 4/22/2022 4:57 PM
	Steps
	
	
	
	He did not specify credentials

	Now we look for synatx of ansible execution
	
The fact that we installed ansible plugin makes it available in the Pipeline Syntax



Screen clipping taken: 4/22/2022 5:46 PM
Steps2


Need to create SSH connection credentials


Screen clipping taken: 4/22/2022 6:02 PM
addCredentials



When you select Kind the options under change

Enter key directly daro.io.pem


Screen clipping taken: 4/22/2022 6:05 PM
privateKey


Button Add

Screen clipping taken: 4/22/2022 6:06 PM
addSSH



Not sure if ssh-add is good enough, we can test it later

We have been in the Pipeline Syntax generating the syntax
	ansiblePlaybook credentialsId: 'private-key', installation: 'Ansible', inventory: 'uat.yml', playbook: 'apache.yml'
	


### CI/CD PIPELINE FOR TODO APPLICATION
### PHASE 2 – INTEGRATE ARTIFACTORY REPOSITORY WITH JENKINS
### ANSIBLE INVENTORY
### SONARQUBE INSTALLATION
### CONFIGURE SONARQUBE
### CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE