# AutomatedIntegration

The name AutomatedIntegration refers to the integration of Git, Jenkins and Docker. 

## Task:
For this project to automate process, We have to make 3 jobs on Jenkins:

#### JOB1:
If Developer push to dev1 branch then Jenkins will fetch from dev and deploy on the dev-docker environment.

#### JOB2:
If Developer push to master branch then Jenkins will fetch from master and deploy on the master-docker environment. As they both dev-docker and master-docker environments are on different docker containers.

#### JOB3:
Manually the QA team will check (test) for the website running in the dev-docker environment. If it is running fine then Jenkins will merge the dev branch to master branch and trigger job 2

### Pre-requisite :
- Operating System: Base OS is Windows 10. Server OS is RedHat Enterprise Linux 8 (RHEL8) in Oracle Virtual Box.
- In RHEL8, the softwares needed are Docker (httpd image downloaded in it), Jenkins (github plugin installed), ngrok program, and also httpd.
- In Windows we need Git Bash software.
- At the starting stop the firewalld in RHEL8 and start the docker and jenkins services.

### Building process:

In this task, I've created a system, where there are two environments. The environments are two docker containers, that are created using Jenkins. The one environment is for Production and other is for Testing. Both the environments are under controll of Jenkins.

Now, using GitBash created one branch as dev1 and considering it as Testing branch and the master branch as Production branch. Commit whatever changes you makein dev1 manually.


- Now first of all, stop the firewall and start the services of docker, jenkins and httpd.
``` 
systemctl stop firewalld
setenforce 0
```
- Give jenkins all the power of Linux:
Edit the `sudoers` file using : `gedit /etc/sudoers` in Redhat.  
Write `jenkins ALL=(ALL)  NOPASSWD:ALL` after the line `root ALL=(ALL) ALL`
![sudoers](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/image.png)
    
- Create the git repository:
First, change to the directory where you want to work.
Then create repository(repo) on github by clicking on 'New Repository'
After creating repo, clone that repo in that folder usiing Git Bash.
`git clone URL-of-your-repo`
This will clone/download that repo in your folder.
Also, you have create some files in master branch using `notepad filename.html` or `cat filename.html`.
 

- Now, Create new branch from Git bash
Like this and change the branch from master to dev1:
![branch](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/WhatsApp%20Image%202020-05-07%20at%2012.07.24%20AM.jpeg)

Add create some files in tha branch using `notepad filename.html` or `cat filename.html`.
and add them to git to commit and push. Following are the commands:
![files](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/add%20files%20in%20dev%20brach.jpeg)

After pushing the files your files will appear on the Github:
![appear](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Screenshot%20(130).png)

- Post-commit:
Now change directory to git hooks to edit the post-commit file.
`cd /.git/hooks/`
edit the post commit file using `notepad post-commit`
add this to the file: 
``` 
 #!/bin/bash
 echo "Welcome to Jenkins!!!"
 git push -u origin dev1
 ```
- As, you have started jenkins from redhat system, get the ip of the system using ` ifconfig enp0s3` .
after getting IP go to the browser type that IP and add port 8080 to it. `190.168....:8080` 
this will start the jenkins GUI.
