# srkdocker
## Docker Commands
```console
export PS1=" "
docker pull busybox
docker run busybox echo "hello from busybox"
docker ps -a
docker run -it busybox sh
docker rm $(docker ps -a -q -f status=exited)
docker run busybox sleep 10 ; echo demo

while sleep 1; do echo "Hi"; done
while true; do echo 'Hit CTRL+C'; sleep 1; done
```
### Docker list all images
```bash
docker image ls
docker images
```
### Docker list all container
```
docker ps -aq
```
### Docker stop all container
```
docker stop $(docker ps -aq)
```
### Docker remove all container
```
docker rm $(docker ps -aq)
```
### Docker remove all images
```
docker rmi $(docker images -q)
#remove none images
docker rmi $(docker images -f dangling=true -q)
```
### Docker mount local volume to container
```
docker run -v /Users/senthilkumarramasamy/Desktop/src/tmp/graphdb:/graphdb -v /Users/senthilkumarramasamy/Desktop/src/tmp/uploads:/uploads  -p 8080:8080 gentics/mesh```
```
### Docker attach to running container
```
docker attach adaa0e2f53df
```
### Docker start a container
```
docker start adaa0e2f53df
```
### Docker create Volume
```
docker volume create my-vol
```
### Docker inspect Volume
```
docker volume inspect my-vol
```
### Docker get logs
```
docker container logs -f $(docker ps -q)
```
### Docker remove Volume
```
docker rm inspect my-vol
```
### Docker list container
```
docker container ls
```
### Docker run bash
```
docker exec -it <ID> bash
docker exec -it $(docker ps -q) bash
```
### Docker run as root user
```
docker run -d -u root
```
### Docker Build Image
```
docker build -t expectedName .
```
## Docker Jenkins
```
docker run -p 9090:8080 -p 50000:50000 jenkins/jenkins
####Volumen Support
docker run -p 9090:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins
```
## Docker accessing host docker socket
```
docker run -d -u root -p 9090:8080 -p 50000:50000 -v /var/run/docker.sock:/var/run/docker.sock -v jenkins_home:/var/jenkins_home jenkins/jenkins
``` 
```
docker run -d -u root -p 9090:8080 -p 50000:50000 -v /var/run/docker.sock:/var/run/docker.sock --privileged -v jenkins_home:/var/jenkins_home -v $(which docker):/usr/bin/docker jenkins/jenkins
```
[Docker Jenkins Host connection](https://getintodevops.com/blog/the-simple-way-to-run-docker-in-docker-for-ci)

[Running Jenkins with root user](https://stackoverflow.com/questions/44444099/how-to-solve-docker-permission-error-when-trigger-by-jenkins)

[Connect Jenkins with Docker Host](https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin)

[Sample Pipeline Code](https://jenkins.io/doc/book/pipeline/docker/)
[Docker not found error](https://forums.docker.com/t/docker-not-found-in-jenkins-pipeline/31683/14)

## Docker start containe automatically on system startup
```
docker run -d --restart=on-failure:5 -v /media/XXX:/XXX -p 80:8080 gentics/mesh
```
## Docker copy files from container
```
docker cp 0bb147c4a140:/workdir/build/tmp/deploy/images/qemux86
https://stackoverflow.com/questions/22049212/copying-files-from-docker-container-to-host
```
## Docker communication
```
docker network ls
docker network inspect
docker run -itd --name=container1 busybox
docker attach container1
docker attach container2
ping -w3 172.17.0.3
```

[Network](https://docs.docker.com/v17.09/engine/userguide/networking/#the-default-bridge-network)

## Docker Yockto
```
/Users/senthilkumarramasamy/workdir
docker run -it -v yoctovolume:/Users/senthilkumarramasamy/workdir gmacario/build-yocto
git clone -b morty git://git.yoctoproject.org/poky.git
```
### Docker User Permission Yockto
```
docker run -it --rm -v yoctovolume:/workdir gmacario/build-yocto 

sudo chown -R build:build /workdir

docker run -it --rm -v yoctovolume:/workdir yocto-srk 
docker run -it --rm -v yoctovolume:/workdir yocto-srk  ./init.sh
docker build -t yocto-srk .

# TODO edit poky todo
https://jenkins.io/doc/book/pipeline/docker/#execution-environment
https://forums.docker.com/t/host-path-of-volume/12277/9
https://forums.xilinx.com/t5/Embedded-Linux/petaLinux-build-linux-donot-use-bitbake-as-root-error/td-p/750023
```
https://zatoichi-engineer.github.io/2017/10/02/yocto-on-osx.html
# Dockerfile commands
## CMD
```
CMD "/bin/bash" 
#command can be used only once in the docker file.
```
[Tutorial](http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)
## Docker busybox tutorial
```
docker run -it --rm busybox
set -e - exit script if any command fails (non-zero value)
exec "$@" - will redirect input variables, see more here
```
## Docker container stop restart
```
docker update --restart=no <CONT ID>
--restart=unless-stopped
docker update --restart=no  $(docker ps -aq)
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
```
##get running container id
```
docker ps -alq
```
# Docker Compose
## Docker Compose commands
```
docker-compose up
docker-compose down
docker-compose up --build XXXX
```
### Docker security
1. https://docs.docker.com/engine/security/security/
***
# Kubernetes
## Kubernetes Commands
```
kubectl run hello-1 --image=busybox --image-pull-policy=Never
UI
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
kubectl proxy
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/
https://medium.com/google-cloud/kubernetes-101-pods-nodes-containers-and-clusters-c1509e409e16
```
Quoting text with markdown
> This is an example of quoting text with markdown.
This is an example of quoting code
```
echo helloworld
```

# Creating Persist Volume
```
kubectl create -f psvolume.yaml
kubectl get pv task-pv-volume
kubectl exec -it task-pv-pod -- /bin/bash
kubectl get pod task-pv-pod
#Run a command inside pod
kubectl exec -it task-pv-pod -- /bin/bash
kubectl create -f ./cronjob.yaml
kubectl get cronjob hello
kubectl get jobs --watch
kubectl delete cronjob hello
```
https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

[Docker Command Ref.](https://docker-curriculum.com)
[Shell Command Shortcuts](https://stackoverflow.com/questions/9679776/how-do-i-clear-delete-the-current-line-in-terminal)

# Kubectl auto complete
```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

# kubectl project
```
kubectl get pods
kubectl exec -it meshtest-deployment-567cb7c866-6jk96 /bin/sh
kubectl proxy
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
```

[Link](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/)

### Command to update deployment
```
kubectl apply -f genetics-mesh.yml
```
### Command to delete deployment
```
kubectl delete deploy meshtest-deployment
```
### Command to get Logs
```
kubectl logs meshtest-deployment-<xxx>
kubectl logs -f meshtest-dev-deployment-f47c474b9-9r9w4 
```
### Command to get pod details
```
kubectl get pods
```
## Delete pods
```
kubectl delete pods --all
```
## Describe pod
```
kubectl describe pod xxxx
```
## Describe pod
```
kubectl delete deploy xxxx
```
## Describe Pod / Deployment
```
kubectl describe deployment
kubectl describe deployment nginx-deployment
```
## Persistent Volume
```
kubectl get pv
```
## Persistent Volume Claim
```
kubectl get pvc
```
## Service
```
kubectl get service 
kubectl get service frontend --watch
kubectl get service meshtest-service --watch
```
## Watch pod status continuously
```
kubectl get pods --watch
```
## kubectl get name space
```
kubectl get namespaces
kubectl --namespace=<insert-namespace-name-here> get pods
kubectl get service --namespace=<insert-namespace-name-here>
```
## Difference between pod and deplooyment
- [Pod and Deployment Difference](https://stackoverflow.com/questions/41325087/in-kubernetes-what-is-the-difference-between-a-pod-and-a-deployment
) 

---
# Mesh
### mesh configure
```
npm install mesh-cli -g
mesh configure
mesh admin index

#URL Format
http://127.0.0.1:8080
```
### Data Backup
```
mesh configure
mesh admin backup
```
Things to backup
  - Graph Database - automatic-backup.json
  - Binary files - data/binaryFiles
  - Elasticsearch Index

Data file structure
  - data
    - binaryFiles
    - graphdb
  - elasticsearch   

https://github.com/gentics/mesh-compose/tree/clustering#online-backup
https://getmesh.io/docs/administration-guide/#_backup_recovery

### EC2 Instance Setup
```
docker run -v /media/mesh-mount/backup:/backup -v /media/mesh-mount/graphdb:/graphdb -v /media/mesh-mount/uploads:/uploads  -p 80:8080 gentics/mesh

docker run -d --restart=on-failure:5 -v /media/mesh-mount/backups:/backups -v /media/mesh-mount/graphdb:/graphdb -v /media/mesh-mount/uploads:/uploads  -p 80:8080 gentics/mesh

#Automatic Docker startup
sudo mkdir -p /etc/systemd/system/docker.service.d
cd sudo  /etc/systemd/system/docker.service.d
cd /etc/systemd/system/docker.service.d
sudo systemctl enable docker

/etc/fstab
/dev/sdb    /media/mesh-mount/    ext4    defaults    0    0

```

### Elastic Search
```
http://ES:9200/_cluster/health
curl http://127.0.0.1:9200/_cluster/health
```
---
# Installing Docker in EC2 Linux instace
```
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
docker info

sudo amazon-linux-extras install docker
sudo systemctl enable docker
sudo usermod -a -G docker ec2-user
#add your user (who has root privileges) to docker group
```
[https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html)

sudo service docker start

https://docs.docker.com/install/linux/linux-postinstall/#configure-docker-to-start-on-boot

```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -    
```
# EC2 Instance Commands
```
mount 
```
```
private key not protected
chmod 400 *.pem
sudo groupadd docker

```
---
# Linux Commands
## systemd
### Start docker on boot
```
sudo systemctl enable docker
```
- [https://docs.docker.com/install/linux/linux-postinstall//](https://docs.docker.com/install/linux/linux-postinstall//)

- [How to create linux service](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6)

## fstab  mount partition at startup

/etc/fstab contains the necessary information to automate the process of mounting partitions.

## Get os version
```
cat /etc/os-release
```

## Linux kernel
```
uname -a
```
### device        mountpoint             fstype    options  dump   fsck
```
/dev/sdb1    /home/yourname/mydata    ext4    defaults    0    0
```
###
```
The third and fourth columns display the file's owning user and group
ls -l
```
### Changing group owner
```
chown -R :<group> <File>
chown -R :<group> <File>
```
#### Ref.

* https://www.thegeekstuff.com/2012/06/chown-examples
---
### AWS User and Group
* https://aws.amazon.com/premiumsupport/knowledge-center/set-change-root-linux/

### tar file linux
```
tar -zcvf tar-archive-name.tar.gz source-folder-name

```
### extract tar file linux
```
tar -zxvf tar-archive-name.tar.gz
```
### SCP Copy file from remote
```
scp -i "xxx.pem" xxxx@xxxxx.com:/media/xxxx.tar.gz ./
```
### Linux USer and group permission
```
# get the user current user details
id
# group id from group name
id -g root
#get owning group
stat -c %G tmp
#get group list
getent group
```
### Docker User and Group
* https://medium.com/@mccode/understanding-how-uid-and-gid-work-in-docker-containers-c37a01d01cf
## Linux
* https://wiki.archlinux.org/index.php/users_and_groups
* https://fideloper.com/user-group-permissions-chmod-apache
---
## Postgres
###
```
sudo -u <USER> createuser user
postgres -V
psql postgres
CREATE ROLE user WITH LOGIN PASSWORD 'demopass'
CREATE DATABASE xxxxx;
grant all privileges on database demouser to "user";
SHOW data_directory;
ps ax | grep postgres | grep -v postgres:
docker run -e POSTGRES_USER=demouser -e POSTGRES_PASSWORD=demopass -e POSTGRES_DB=demo library/postgres


```
## postgress docker start
https://stackoverflow.com/questions/37694987/connecting-to-postgresql-in-a-docker-container-from-outside

### Insert data in table
```
INSERT INTO table(column1, column2, …)
VALUES
 (value1, value2, …);
```
# AWS

- Amazon Elastic Container Service 
- EC2
- S3
- EBS
---
[Git rename branch](https://multiplestates.wordpress.com/2015/02/05/rename-a-local-and-remote-branch-in-git/)
# Visual Code
    - shell command
    - brew install gettext
    - brew link --force gettext
     
---
# Git 
## Git Branch
```
git branch <feature_branch>
git checkout <feature_branch>
git push origin <feature_branch>
git push --set-upstream origin <feature_branch>
```
## Git Stash
```
git stash show -p stash@{0} | git apply -R
```
## git Stash POP
```
# The changes are saved with stash and returned with stash pop. Works even when the branch is changed.
git stash
git stash pop
git stash pop is git stash apply && git stash drop
```
## Git Stash to Branch
```
git stash branch <name>
```
## git rename branch
```
git branch -m new-name
git push origin :old-name new-name
git push origin -u new-name
```

[git braching](https://confluence.atlassian.com/bitbucket/branching-a-repository-223217999.html)

# RISC Instruction set
- [RISCV Instruction Set](https://content.riscv.org/wp-content/uploads/2017/05/riscv-spec-v2.2.pdf)


# Python3 mac
```
brew install python3
pip install virtualenv
virtualenv srkpython3
source srkpython3/bin/activate
pip install Django 
```
***
# openssl certificate generation
### 
```
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out cert.pem
openssl x509 -text -noout -in cert.pem
``` 

# English 
text excerpt

List Example

> This is a quote example

Use `git status` to list all new or modified files that haven't yet been committed.


- One
- Two
- Three

* One
* Two
* Three

1. One
2. Two
3. Three

- [x] My First task
- [ ] My Second task
- [ ] My Third task

# Bold
**Bold Text**

# Italic
*Italic text*

# Strike Through

~~This is a stike through text~~
