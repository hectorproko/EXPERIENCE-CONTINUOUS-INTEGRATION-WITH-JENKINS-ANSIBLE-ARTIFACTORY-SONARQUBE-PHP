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
**Settings** > **Developer settings** > **Personal access tokens** , button **Generate new token**  
Under **Note** `"jenkins-access-token"read:user`  
**Select Scopes** we put **repo** and on **user** select `user:email user, read:user`   


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/token.png)  

`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`  



![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/storeCode.png)  


Created repo https://github.com/hectorproko/ansible-project  

At this point it tries to find a **Jenkinsfile** does not find it and prompts you to a page to generate one  

I'll ignore it and click **Administration**  

Now in the Dashboard we see the (newly created pipeline) job. It takes the name of your GitHub repository **ansible-project**  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/pipeline.png)  

Created a **Jenkinsfile** (inside dir `deploy` in our repo) that I will gradually build by first adding a **Build** stage with this code snippet  
    
``` bash
pipeline {
    agent any
stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }
    }
}
```


Now **ansible-project** job has additional options     

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/ansibleProject.png)  


I will change the config of the job by clicking button **Configure**  

click tab **Build Configuration**    
Here I specify the path of the Jenkinsfile    

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/buildConfig.png)  


Click **Save**  

I'm prompted back to **ansible-project** we get to pick branches we pick the only one **main**  

Click **Build Now** to test  


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/branchMain.png)  

If we check the **Console Output** of this build we see the intended message  
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


I just triggered the job from regular Jenkins GUI we could do it from  Open Blue Ocean as well     
The log from Ocean Blue is easier on the eyes      


![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/build.png)  


To appreciate the **multibranch** options I'll create another branch and name it `feature/jenkinspipeline-stages`  
In this new branch the **Jenkinsfile** will have an additional stage **Test**  
``` bash
stage('Test') {
  steps {
    script {
      sh 'echo "Testing Stage"'
    }
  }
}
```

For the new branch to show up Navigate to the ansible-project dashboard and click on "**Scan repository now**"  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/scanRepo.png)  


In Blue Ocean, I can now see how the ``Jenkinsfile`` caused a new step to appear in the pipeline  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/niceInterface.png)  


Took this idea further and created more stages in `feature/jenkinspipeline-stages`  

Tested the branch with new stages in Blue Ocean branch `feature/jenkinspipeline-stages`  

Created a pull request to merge `feature/jenkinspipeline-stages` to `main`  

Tested the job in **Ocean Blue** using `main`. I can see additional stages displayed
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/ansibleProject8.png)  


### RUNNING ANSIBLE PLAYBOOK FROM JENKINS  

Install ansible plug-in  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/ansiblePlugin.png)  


**Manage Jenkins** > **Global Tool Configuration**  

I point Jenkins to my Ansible installation  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/addAnsible.png)  

Finding my Jenkins installation path  
``` bash 
ubuntu@ip-172-31-94-159:~$ which ansible
/usr/bin/ansible
```

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/addAnsible2.png)  

I used the **Pipeline Syntax** link to go to the **Snippet Generator** to generate the code I put in the **Jenkinsfile**  

I start with creating **SSH connection credentials**  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/addCredentials.png)  

I enter the contents of my **.pem** key directly  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/privateKey.png)  

Button **Add**  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/addSSH.png)  


I'll put the **ansible.cfg** file alongside **Jenkinsfile** in the deploy directory.  
	
<!--- This is an HTML comment in Markdown 
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
-->
	
The goal is for **ansible-project** job (pipeline) to run `site.yml`


The pipeline configuration lives in a repo so all the job **ansible-project** does is download the instructions and execute them. The configuration itself doesn’t exist in Jenkins  
	
For now I'm going to parameterize (ask for input) part of the pipeline code  
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