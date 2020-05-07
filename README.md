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

Now first of all, stop the firewall and start the services of docker, jenkins and httpd.
