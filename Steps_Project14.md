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
	
For now I'm going to parameterize (ask for input) part of the pipeline code using snippet   
``` bash
pipeline {
    agent any	
	parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
```

Example of parameter when running job in Blue Ocean  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/inputRequire.png)  


	
OK: here we are trying to do it with site.yml

		

Seems like we need to modify ansible-project (is there I have my Jenkinfifle)to run different stages one of them cloning ansible-config-mgnt (he doesn’t have this name, I don’t have deploys here with jenkinsfile and ansible.cf). Running playbook site.yml, making sure we can dynamically change inventory
		
Ànsible-project repo has the jenkinsfile with the pipeline code and ansible.cnf to donwload ansible-mngt
		
The way he has it ansible-config has the site.yml and jenkinsfile
		
<!---	
Tested the parematization from anisble-demo worked
The job now shows Build with Parameters where you can put, in my case the whole file dev.ini or uat.ini
-->
		
<!---		
Not used anymore
https://github.com/hectorproko/ansible-project
-->		

ansible-project job now has repo https://github.com/hectorproko/ansible-config-mgt
		
Uploaded ansible.cfg
https://github.com/hectorproko/ansible-config-mgt/blob/main/deploy/ansible.cfg
		
		
So the ansible.cfg can't have roles_path because we are trying to generate a path relative to the Jenkins workspace which changes with branch name. If it already contains a role_path you get error
		
Creating PullRequest to going **HisJekinsfile** to **main**

### CI/CD PIPELINE FOR TODO APPLICATION

Install **Antifactory** manually, need to check roles version

``` bash
#Install Gnupg2 package
sudo apt-get install gnupg2 -y
#Next, download and add the GPG key(had to switch to user root to install sudo wasn’t enough)
sudo wget -qO - https://api.bintray.com/orgs/jfrog/keys/gpg/public.key | apt-key add -
#Add the JFrog Artifactory repository
echo "deb https://jfrog.bintray.com/artifactory-debs bionic main" | tee /etc/apt/sources.list.d/jfrog.list

```

Once the repository is added, update the repository and install JFrog Artifactory with the following command:
	apt-get update -y
	apt-get install jfrog-artifactory-oss -y
Next, start the Artifactory service and enable it to start at system reboot with the following command:
	systemctl start artifactory
systemctl enable artifactory
	
Next, verify the status of Artifactory service using the following command:
	systemctl status artifactory


Install PHP
    sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}


	Issue:
		Ansible-Jenkins instance stopped working after installing artifactory
		Created an image from messed up instance
https://docs.bitnami.com/aws/faq/administration/clone-server/
		Was able to see the jenkins website after a while trying to see it again after restarting, still nothing. So the instance is very slow.
		Plan is to copy the jenkins folder to another isntance and saves all the jenkins jobs at least, using git
		
	Solution:
		Using the image AMI I created I launched a new instance of type t2.small, seems like it was a memory thing Might have to go all the way to t2.medium
	
	
	
Forked  https://github.com/darey-devops/php-todo.git to
https://github.com/hectorproko/php-todo
	
Installed
 sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}

Install Comoposer
Extra tutorial https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-ubuntu-20-04
https://getcomposer.org/download/

At some point used (sudo apt install composer I think it was wrong to do that)

ubuntu@ip-172-31-22-63:~$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
ubuntu@ip-172-31-22-63:~$ ls
2plays.yml  ansible-config-artifact  common.yml  composer-setup.php  daro.io.pem  daro.ioJenkins.pem  hello  key  snap
ubuntu@ip-172-31-22-63:~$ php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
Installer verified
ubuntu@ip-172-31-22-63:~$ php composer-setup.php
All settings correct for using Composer
Downloading...

Composer (version 2.3.5) successfully installed to: /home/ubuntu/composer.phar
Use it: php composer.phar





Screen clipping taken: 4/25/2022 9:02 AM
plot


Screen clipping taken: 4/25/2022 9:04 AM
artifactory

Configure the server ID, URL and Credentials, run Test Connection.
Manage Jekins > Configure System


Screen clipping taken: 4/25/2022 9:19 AM
jfrogAdd


Screen clipping taken: 4/25/2022 9:48 AM
jfrogPlatform

I have a user jerkins and can log in with password 0679

	Issue:
		An error occurred while connecting to JFrog Artifactory:
		JFrog service failed. Received 401: {
		  "errors" : [ {
		    "status" : 401,
		    "message" : "Bad credentials"
		  } ]
		}
		An error occurred while connecting to JFrog Distribution:
		JFrog service failed. Received 403: {
		  "errors" : [ {
		    "status" : 403,
		    "message" : "This request is blocked due to recurrent login failures, please try again in 5 seconds"
		  } ]
		}
	
	Opened Port 8082, nothing
	Seems like the user is to log in to artifacotry a user of artifactory

	
	Screen clipping taken: 4/25/2022 9:54 AM
	welcome
	
	Used default credentials found
https://www.shellhacks.com/jfrog-artifactory-default-password-test-credentials/
	Prompted me to change them once log in
	
	
	Skipped Set based URL, Configure Default Proxy, Create Repositories
	
	In Identity and Access > Users created a new user hector 0679 changed to G0dBless!
	
	Changed password after forgetting:
	https://www.jfrog.com/confluence/display/JFROG/Users+and+Groups#ManagingUsers-RecreatingtheDefaultAdminUser
	
	
	Screen clipping taken: 4/25/2022 10:32 AM
	users
	
	Now we put this user in Jenkins
	Remember we need to open port 8082, at least for that initial user creation?
	
	
	Screen clipping taken: 4/25/2022 10:52 AM
	jfrogPlatform2
	
	
	JFrog Distribution is a complementary product to JFrog Artifactory and is run as a separate installation as a set of microservices. 
	
	


### PHASE 2 – INTEGRATE ARTIFACTORY REPOSITORY WITH JENKINS
### ANSIBLE INVENTORY
### SONARQUBE INSTALLATION
### CONFIGURE SONARQUBE
### CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE