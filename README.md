Intro
===
This project aims to show the common DevOps software development flow.

In this typical scenario any change to the software base will be recognized and a pipeline software will do the necessary test before deploying the new artifact in the running environment.

Pre-requisites
===
Vagrant

Implementation details
===
This PoC use the follow components:
- Vagrant
- Ansible
- Minikube
- Jenkins
- A simple Python app

Vagrant is used to deploy a virtual machine with inside a minikube installation.
I put some effort to fully automate the process. No manual activity is needed to see your minikube cluster Up&Running.
Inside Kubernetes a Jenkins server is launched with some predefined jobs to check for changes in the repo git@github.com:zioalex/hello_world.git .
After a change has been detected and the test passed successfully a new container is created and a new Kubernetes deployment is started.
In this example the internal Minikube Docker regitry is used.

The Jenkins home is configured in a persistent volume therefore your jobs will survive any Jenkins disruption.

How to start it
===
Test setup
---
    Ubuntu 18.04.4 LTS
    Vagrant 2.2.5
    VirtualBox 6.0.22

Test it
---
Check me out
    git checkut --submodules
    cd vagrant
    vagrant up minikube

Be patient! The whole process will take ~12-15 mins

Update the your ssh config and run a ssh port forward
    vagrant ssh-config >> ~/.ssh/config
    ssh minikube -L 30010:172.17.0.3:30000
    ssh minikube -L 30011:172.17.0.3:30001

Jenkins will be avaibale on http://localhost:30010
while the app on http://localhost:30011

A Vagrant port forwaring has been configured but it seems to be not so stable.
However if it works you can find the services in the follow urls:
Jenkins will be avaibale on http://localhost:30000
while the app on http://localhost:30001

The default Jenkins credential are admin/admin .
    
What I've (re/)learned
===
- How to start an unattended jenkins installation with predefined plugins and password
- How to backup/restore Jenkins jobs with jenkins-cli.jar
- How to create secrets in Jenkins with jenkins-cli.jar
- How to connect Jenkins with Github
- A bit of Ansible
- How to access the Minikube's Docker host from inside Kubernetes
- Kubernetes Kustomization to create Secrets and ConfigMaps

TODO
===
- Improve ansible code
- Better unittest for the Python app
- Use helm chart to deploy
- Send Slack notifications
- Writing Jenkins Jobs as code
- Better organize the code in the submodule
- Make it better
