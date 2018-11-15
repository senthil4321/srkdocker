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
```
### Docker mount local volume to container
```
docker run -v /Users/senthilkumarramasamy/Desktop/src/tmp/graphdb:/graphdb -v /Users/senthilkumarramasamy/Desktop/src/tmp/uploads:/uploads  -p 8080:8080 gentics/mesh```
```
###docker attach to running container
```
docker attach adaa0e2f53df
```
###docker start a container
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
### Docker remove Volume
```
docker rm inspect my-vol
```
### Docker list container
```
docker container ls
```
##Docker Jenkins
```
docker run -p 9090:8080 -p 50000:50000 jenkins/jenkins
####Volumen Support
docker run -p 9090:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins

```
## Docker start containe automatically on system startup
```
docker run -d --restart=on-failure:5 -v /media/XXX:/XXX -p 80:8080 gentics/mesh
```
## Docker Yockto
```
/Users/senthilkumarramasamy/workdir
docker run -it -v yoctovolume:/Users/senthilkumarramasamy/workdir gmacario/build-yocto
git clone -b morty git://git.yoctoproject.org/poky.git
```
## Docker User Permission Yockto
```
$ docker run -it --rm -v yoctovolume:/workdir gmacario/build-yocto sudo chown -R build:build /workdir
```
## Docker copy files from container
```
docker cp 0bb147c4a140:/workdir/build/tmp/deploy/images/qemux86
```
https://zatoichi-engineer.github.io/2017/10/02/yocto-on-osx.html
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
---
# Mesh
### mesh configure
```
npm install mesh-cli -g
mesh configure
mesh admin index
```

# Installing Docker in EC2 Linux instace
```
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user
docker info
```
[https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html)

# EC2 Instance Commands
```
mount 
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
# device        mountpoint             fstype    options  dump   fsck

/dev/sdb1    /home/yourname/mydata    ext4    defaults    0    0
---
# AWS
- Amazon Elastic Container Service 
- EC2
- S3
- EBS
 
---
List Example

- One
- Two
- Three

* One
* Two
* Three

1. One
2. Two
3. Three


# Bold
**Bold Text**

# Italic
*Italic text*

# Strike Through

~~This is a stike through text~~

https://www.imperialviolet.org/2016/12/31/riscv.html
https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwj-ztGLgbbeAhXE0aQKHfUjAgQQFjAAegQICRAC&url=https%3A%2F%2Fcontent.riscv.org%2Fwp-content%2Fuploads%2F2017%2F05%2Friscv-spec-v2.2.pdf&usg=AOvVaw3GMreXNki5Yuy0GlikEg51
### Difference between pod and deplooyment
https://stackoverflow.com/questions/41325087/in-kubernetes-what-is-the-difference-between-a-pod-and-a-deployment





	