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
#Update the repository
apt-get update -y
#Install JFrog Artifactory
apt-get install jfrog-artifactory-oss -y
#start the Artifactory service and enable it to start at system reboot with the following command:
systemctl start artifactory
systemctl enable artifactory
#Verify the status of Artifactory
systemctl status artifactory

```

Install **PHP**
``` bash
sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}
```

`Issue:`  
Ansible-Jenkins instance stopped working after installing artifactory. Created an image from messed up instance  
`Solution:`  
Using the image AMI I created I launched a new instance of type t2.small, seems like it was a memory thing Might have to go all the way to **t2.medium**  
	
`https://github.com/hectorproko/php-todo`  
	
Install PHP, its dependencies and Composer tool  
``` bash
sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}
```
[Install Composer](https://getcomposer.org/download/)  
 

``` bash
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
```
Install Jenkins plugins  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/plot.png)  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/artifactory.png) 


Configure the server ID, URL and Credentials, run Test Connection.  
**Manage Jenkins** > **Configure System**  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/jfrogAdd.png)  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/jfrogPlatform.png)  

I have a user `jenkins` and can log in with password `0679`  

	
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/welcome.png)  
	
Used default credentials: user `admin` password `password`. Once prompted changed default credentials  
	
Skipped **Set based URL**, **Configure Default Proxy**, **Create Repositories**  
	
In **Identity and Access** > **Users**  
Created a new user ``hector``  

<!-- 
	Changed password after forgetting:
	https://www.jfrog.com/confluence/display/JFROG/Users+and+Groups#ManagingUsers-RecreatingtheDefaultAdminUser
-->
	
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/users.png)  

	
Now we put this user in Jenkins
Remember to open port 8082  
	
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/jfrogPlatform2.png)  

`JFrog Distribution` is a complementary product to `JFrog Artifactory` and is run as a separate installation as a set of microservices.  


### INTEGRATE ARTIFACTORY REPOSITORY WITH JENKINS

Created `Jenkinsfile` in https://github.com/hectorproko/ansible-project/tree/main/deploy (changed repo) with snippet:   
``` bash
	pipeline {
    agent any
	stages {
	stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }
	stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/hectorproko/php-todo.git'
      }
    }
	stage('Prepare Dependencies') {
      steps {
             sh 'mv .env.sample .env'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}
```

On the database server (NFS), created database `homestead` and user `homestead`  

``` MySQL
[ec2-user@ip-172-31-81-201 ~]$ sudo mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.26 Source distribution

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| tooling            |
+--------------------+
5 rows in set (0.03 sec)

mysql> Create database homestead;
Query OK, 1 row affected (0.02 sec)

mysql> CREATE USER 'homestead'@'%' IDENTIFIED BY 'sePret^i';
Query OK, 0 rows affected (0.03 sec)

mysql> GRANT ALL PRIVILEGES ON * . * TO 'homestead'@'%';
Query OK, 0 rows affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| homestead          |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| tooling            |
+--------------------+
6 rows in set (0.01 sec)

mysql>
```
Updated the database connectivity requirements in the file `.env.sample`
``` bash 
DB_HOST=127.0.0.1 #Need to put remote server
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=sePret^i
```

<!-- Not sure how to phrase this  -->
sudo apt install composer

compose need to for this to work
``` bash
+ php artisan migrate
Migration table created successfully.
Migrated: 2014_10_12_000000_create_users_table
Migrated: 2014_10_12_100000_create_password_resets_table
Migrated: 2015_10_27_141258_create_tasks_table
[Pipeline] sh
+ php artisan db:seed
[Pipeline] sh
+ php artisan key:generate
Application key [Vvnp0EfkhlkxAyTTjHI5Xggp0U9NE6In] set successfully.
```


Ansible-jenkins instance did not have `mysql-client`, installed it
``` bash
sudo apt install mysql-client-core-8.0
```


**Stage unit test**  
Doesn’t seem to be doing anything

**Stage code analysis** 
For PHP the most commonly tool used for code quality analysis is phploc. Read the article here for more
The data produced by phploc can be ploted onto graphs in Jenkins.

Add the code analysis step in `Jenkinsfile`. The output of the data will be saved in `build/logs/phploc.csv` file.

Works and generates the file 
``` bash
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'
}
}
```

**stage('Plot Code Coverage Report')**  
Don’t know what this is doing
Now we see the button Plot on the job, as  result of putting this piece of code in the Jenkinsfile  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/menuplot.png)

[Jenkinsfile](https://github.com/hectorproko/ansible-project/tree/main/deploy)

So turns out we need a a different job because we will be calling **ansible-project**
Phase 2 deals with [php-todo](https://github.com/hectorproko/php-todo) so going to create the Jenkins file inside that repo and create job of the same name. we have to delete the branch artifactory from **ansible-project** cuse that Jenkinsfile is going to **php-todo** repo

Creating job in Jenkins named php-todo  
Job needs to be configured from Ocean Blue  
Or from the regular place and pick **Multibranch Pipeline** the job now has the tab **Build Configuration**. So each type of New Item has different Tabs.

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/multibranch.png)

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/general.png)

Need to create the job through ocean blue cuse there is a step that asks for Jenkins token, not sure how to do it with regular job. Scratch that even thought getting error validation button worked. It disappear afterwards

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/branchSources.png)

Repo came with 4 branches in total including **main**

We see a zip file from stage package
``` bash
stage ('Package Artifact') {
  steps {
    sh 'zip -qr ${WORKSPACE}/php-todo.zip ${WORKSPACE}/*'
}
```

``` bash
ubuntu@ip-172-31-89-170:/var/lib/jenkins/workspace$ ls php-todo_main
ExcludeFiles  LICENSE.txt  app      bootstrap  build.xml      composer.lock  dat
abase     package.json  phpunit.xml  resources   sonar.properties  tests
Jenkinsfile   README.md    artisan  build      composer.json  config         gul
pfile.js  php-todo.zip  public       server.php  storage           vendor
```

`php-todo.zip  `


So we pushed the stage that uploads the artifact, but there is not repo to upload to yet. Need to create it in artifactory
Going to access artifacotry `http://3.220.20.204:8082/`, Elastic IP and needed port, I had created a user hector already`

Publish the resulted artifact into Artifactory
``` bash
		stage ('Upload Artifact to Artifactory') {
		          steps {
		            script { 
		                 def server = Artifactory.server 'artifactory-server'                 
		                 def uploadSpec = """{
		                    "files": [
		                      {
		                       "pattern": "php-todo.zip",
		                       "target": "<name-of-artifact-repository>/php-todo",
		                       "props": "type=zip;status=ready"
		
		                       }
		                    ]
		                 }""" 
		
		                 server.upload spec: uploadSpec
		               }
		            }
		
		        }
```
  




Created repository
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/createRepo.png)

I think is local  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/localRepo.png)  

Select Package Type  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/packageType.png)  

Only gives me these 4 options


Going with genetic cuse it is zip file

Named it **php-todo-Repo**  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/repoKey.png)  


Button **Create Local Repository**

**Message:**  
``` http
All repositories were created successfully
Your next step is to add users who can access the repository
```
Button: **Add Users**
**Skipped** we already created a hector user with **Administer Platform** role

Now that we have repo let's put the identifier of the repo in the code/stage
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/2repos.png) 

Piece of code on **upload stage**  
`	"target": "php-todo-Repo/php-todo",	`
	
Now we Confirm the **Artifact** is there in the repo  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/application.png)  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/phptodo.png)  

Can also use the path of the URL `http://3.220.20.204:8082/ui/repos/tree/General/php-todo-Repo`

Added stage ('Deploy to Dev Environment')  

``` bash
		stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-project/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
```
**php-todo** executed succesfully and this stage with execute another job **ansible-project** and pass it the parameter needed. Aka the inventory or env

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/phptodo26.png)  

Now **php-todo** job does through all the stages and triggers **ansible-project** at the end  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/consoleOutput.png)  



### ANSIBLE INVENTORY
Nothing here  
### SONARQUBE INSTALLATION
Just followed the steps  

Provisioning instance  
c4x.large Ubuntu  
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/EXPERIENCE-CONTINUOUS-INTEGRATION-WITH-JENKINS-ANSIBLE-ARTIFACTORY-SONARQUBE-PHP/main/images/sonarqube.png)  

### CONFIGURE SONARQUBE


**SONARQUBE INSTALLATION**  
Before we start getting hands on with **SonarQube** configuration, it is incredibly important to understand a few concepts:  
	• *Software Quality* – The degree to which a software component, system or process meets specified requirements based on user needs and expectations.  
	• *Software Quality Gates* – Quality gates are basically acceptance criteria which are usually presented as a set of predefined quality criteria that a software development project must meet in order to proceed from one stage of its lifecycle to the next one. 

[Software Quality Gates](https://docs.sonarqube.org/latest/user-guide/quality-gates/)  

SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.  

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as [The Sonar Way](https://docs.sonarqube.org/latest/instance-administration/quality-profiles/) ). Software testers and developers would normally work with project leads and architects to create custom quality gates.  

**Install SonarQube on Ubuntu 20.04 With PostgreSQL as Backend Database**  
Here is a manual approach to installation. Ensure that you can to automate the same with Ansible.  
Below is a step by step guide how to install **SonarQube 7.9.3** version. It has a strong prerequisite to have Java installed since the tool is Java-based. MySQL support for SonarQube is deprecated, therefore we will be using PostgreSQL.  

We will make some Linux Kernel configuration changes to ensure optimal performance of the tool – we will increase `vm.max_map_count`, `file discriptor` and `ulimit`.  

**Tune Linux Kernel**  
This can be achieved by making session changes which does not persist beyond the current session terminal. (Seems like the first two are made permanent in /etc/sysctl.conf)

``` bash
sudo sysctl -w vm.max_map_count=262144  #it writes to `sysctl.conf`)
sudo sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 262144
```
To make a permanent change, edit the file `/etc/security/limits.conf` and append the below  
sonarqube   -   nofile   65536 (these 2 are supposed to be the permanent version of the first 2 sudo sysctl)
sonarqube   -   nproc    4096

Before installing, let us update and upgrade system packages:
``` bash
sudo apt-get update -y
sudo apt-get upgrade -y
```
Install **wget** and **unzip** packages
``` bash
sudo apt-get install wget unzip -y
```
Install OpenJDK and Java Runtime Environment (JRE) 11
``` bash
sudo apt-get install openjdk-11-jdk -y
sudo apt-get install openjdk-11-jre -y
```
Set default JDK – To set default JDK or switch to OpenJDK enter below command:
``` bash
sudo update-alternatives --config java
```
If you have multiple versions of Java installed, you should see a list like below:
```
ubuntu@ip-172-31-89-89:~$  sudo update-alternatives --config java
There is only one alternative in link group java (providing /usr/bin/java): /usr/lib/jvm/java-11-openjdk-amd64/bin/java
Nothing to configure
```

Seem like a database user was created. Correct **createuser** is a postgress command not a linux one so we are just creating a **role** sonar in the database.


Taking note of version:  
``` bash
ubuntu@ip-172-31-89-89:~$ java -version
openjdk version "11.0.15" 2022-04-19
OpenJDK Runtime Environment (build 11.0.15+10-Ubuntu-0ubuntu0.20.04.1)
OpenJDK 64-Bit Server VM (build 11.0.15+10-Ubuntu-0ubuntu0.20.04.1, mixed mode, sharing)
ubuntu@ip-172-31-89-89:~$f
```
**Install and Setup PostgreSQL 10 Database for SonarQube**

The command below will add PostgreSQL repo to the repo list:  
``` bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```
Download PostgreSQL software  
``` bash
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -  
```
Install PostgreSQL Database Server  
``` bash
sudo apt-get -y install postgresql postgresql-contrib
```
Start PostgreSQL Database Server  
``` bash
sudo systemctl start postgresql
```
Enable it to start automatically at boot time  
``` bash
sudo systemctl enable postgresql
```
Change the password for default postgres user (Pass in the password you intend to use, and remember to save it somewhere)  
``` bash
sudo passwd postgres
```
Switch to the `postgres` user  
``` basg
su - postgres
```
Create a new user by typing  
``` bash
createuser sonar
```
Switch to the PostgreSQL shell  
``` bash
psql
```
Set a password for the newly created user for SonarQube database  
```sql
ALTER USER sonar WITH ENCRYPTED password 'sonar';
```  
Create a new database for PostgreSQL database by running:
```sql
CREATE DATABASE sonarqube OWNER sonar;
```
Grant all privileges to sonar user on sonarqube Database.
```sql
grant all privileges on DATABASE sonarqube to sonar;
```
Exit from the psql shell:
```sql
\q
```
Switch back to the sudo user by running the exit command.
``` sql
Exit
```
**Install SonarQube on Ubuntu**  
Navigate to the `tmp` directory to temporarily download the installation files  
``` bash
cd /tmp && sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.9.3.zip
```

Unzip the archive setup to `/opt directory`  
``` bash
sudo unzip sonarqube-7.9.3.zip -d /opt
```
Move extracted setup to `/opt/sonarqube` directory (rename)
``` bash
sudo mv /opt/sonarqube-7.9.3 /opt/sonarqube
```




### CONFIGURE SONARQUBE AND JENKINS FOR QUALITY GATE



<details close>
<summary>Expand GIF</summary>

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/main/images/tooling_project_15.png)  
</details>

