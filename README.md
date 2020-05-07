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

Also, make two directories in Redhat, named Production and Testing.
    
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
after getting IP go to the browser type that IP and add port 8080 to it. `192.168....:8080` 
this will start the jenkins GUI.
Log in to your Jenkins account.
If account is not set, 
run ` cat /var/lib/jenkins/secrets/initialAdminPassword ` command in your redhat terminal, and copy that password and paste into that inputbox. You'll be loged In. Install the Github Plugins from there. Now, you'll be asked to set new credentials. Complete that step.
![jenkin](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/jenkin%20setup.jpeg)

- Now, Click on create new job and start configuring new job:
![job1](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/job1_1.JPG)
![job11](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job1_2.JPG)
```
if sudo docker ps | grep production
then
echo "Prod server already running"
else
sudo docker run -d -t -i -p 8082:80 -v /root/Production:/usr/local/apache2/htdocs/ --name production httpd
fi
sudo cp -r -v -f * /root/Production/
```
![job111](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job1-3.JPG)

Click on Apply and then Save.

- Proceed for Job 2:
![job2](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job21.JPG)

![job2](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job22.JPG)

```
if sudo docker ps | grep testing
then
echo "Test server already running"
else
sudo docker run -d -t -i -p 8081:80 -v /root/Testing:/usr/local/apache2/htdocs/ --name testing httpd
fi
sudo cp -r -v -f * /root/Testing/
```

![job2](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job23.JPG)

Click on Apply and then Save.

- Proceed for Job3 (Merging):

![job3](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job31.JPG)

![job3](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job32.JPG)

![job3](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Job33.JPG)

Click on Apply and then Save.


- Now, Build all the jobs
and in your browser, type `192.168.****:8081 ` and `192.168.****:8082` and browse the files of your testing and production server.
In this task, my 8082 port is for production and 8081 for testing.
You can now see the the production files in the testing server and testing files in the production server.
Examples:
    - Production server
![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Production%20server.JPG)
    - Testing server
![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Screenshot%201.JPG)
    - production files in the testing server
![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Prod%20files%20on%20testing%20server.JPG)
    - Testing files in the production server
![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Production%20server%20with%20testing%20files.JPG)


![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/Buildsuccess.JPG)

- NGROK deployment

Run `./ngrok httpd 8082` command in the  folder where your ngrok program is located.

![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/ngrok.JPG)
![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/ngrok%20url.JPG)
![image](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/Snapshots/ngrok%20test.JPG)

This ngrok url can be shared and can be accsessed from any device. 


# Author:

### [Vinuja Khatode](https://github.com/vinujakhatode/)

### [Liscense](https://github.com/vinujakhatode/AutomatedIntegration/blob/master/LICENSE)
