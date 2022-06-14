# EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP
PROJECT 14


### ANSIBLE ROLES FOR CI ENVIRONMENT

#### Installing roles **Skipped**



Install & Open **Blue Ocean Jenkins** Plugin  
**Manage Jenkins** > **Plugin Manager**   

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/blueOcean.png)  

Now I can see the Blue Ocean icon in the **Dashboard**  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/dashboard.png)  


Creating **Access Token** in Github  
**Settings** > **Developer settings** > **Personal access tokens** , button Generate new token  
Under **Note** `"jenkins-access-token"read:user`  
**Select Scopes** we put **repo** and on **user** select `user:email user, read:user`   


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
	
	Step:
	You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory.
	
	Till now we have used Pipeline ansible-demo

``` bash
	pipeline {
	    agent any
	
	    stages {
	        stage('SCM Checkout') {
	            steps {
	                git 'https://github.com/hectorproko/myapp-ansible.git'
	            }
	        }
	        stage('Execute Ansible') {
	            steps {
	                ansiblePlaybook credentialsId: 'private-key', installation: 'Ansible', inventory: 'uat.ini', playbook: 'apache.yml'
	            }
	        }
	    }
	}
```
	
	Need to run site.yml with the roles

	Seems like we need to take ansible-project job and turn into pipeline and run site.yml
	Ìt is already pipeline Blue ocean configuraiton is weird
	Remember the pipeline configuraiton is in a repo so all we are doing is downloading it doesn’t live in Jenkins for ansible-project for ansible-demo it does
	
	For now we are going to take 
	Doing parameterization with ansible-demo
	Worked:
	Adding
``` bash
		pipeline {
    agent any	
		parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
```

	When you click Run it prompts you (Blue Ocean Plug in)

		Screen clipping taken: 4/26/2022 9:36 AM
		inputRequire
		
	Without Blue Ocean Plug-in on the the regular Configure you see. Under tab General


	Screen clipping taken: 4/26/2022 9:38 AM
	stringParameter
	
	
	OK: here we are trying to do it with site.yml
		Ok I just tried to run the playbook from Jenkins job and it worked without ssh-add. I believe after u restart the instance you lose the ssh-add and need to re-add so the fact that i did not do that just now and it worked Im assuming it worked without it and it relied solely on the private key configured in the job
		
		Using Taiwo repo as referemcet
		https://github.com/Taiwolawal/ansible-config/blob/main/deploy/Jenkinsfile
		
		
		Seems like we need to modify ansible-project (is there I have my Jenkinfifle)to run different stages one of them cloning ansible-config-mgnt (he doesn’t have this name, I don’t have deploys here with jenkinsfile and ansible.cf). Running playbook site.yml, making sure we can dynamically change inventory
		Ànsible-project repo has the jenkinsfile with the pipeline code and ansible.cnf to donwload ansible-mngt
		
		The way he has it ansible-config has the site.yml and jenkinsfile
		
		
		We are moving ansible-project jenkinsfile to ansible-config-mgnt
		
	
		Tested the parematization from anisble-demo worked
The job now shows Build with Parameters where you can put, in my case the whole file dev.ini or uat.ini
		ansiblePlaybook credentialsId: 'private-key', installation: 'Ansible', inventory: '${WORKSPACE}/${inventory}', playbook: 'apache.yml'	
		
		
		Not used anymore
		https://github.com/hectorproko/ansible-project
		
		 ansible-project job now has repo https://github.com/hectorproko/ansible-config-mgt
		
		Uploaded ansible.cfg
		https://github.com/hectorproko/ansible-config-mgt/blob/main/deploy/ansible.cfg
		
		
		Executed site.yml manually specifying the directory. Not sure why  I need to specify directory they had their on file that did that. But the job in jenkins has to put an env which is an inventory file wth "-i" when jenkins urn command

		So the ansible.cfg can't have roles_path because we are trying to generate a path relative to the Jenkins workspace which changes with branch name. If it already contains a role_path you get his error
		
Creating PullRequest to going HisJekinsfile to main

### CI/CD PIPELINE FOR TODO APPLICATION
### PHASE 2 – INTEGRATE ARTIFACTORY REPOSITORY WITH JENKINS
### ANSIBLE INVENTORY
### SONARQUBE INSTALLATION
### CONFIGURE SONARQUBE
### CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE