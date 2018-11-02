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

##Kubernetes Commands
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

#Creating Persist Volume
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


#Kubectl auto complete
```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

#kubectl project
```
kubectl get pods
kubectl exec -it meshtest-deployment-567cb7c866-6jk96 /bin/sh
kubectl proxy
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
```
###Command to update deployment
```
kubectl apply -f genetics-mesh.yml
```
###Command to delete deployment
```
kubectl delete deploy meshtest-deployment
```
###Command to delete deployment
```
kubectl logs meshtest-deployment-<xxx>
```
###Command to get pod details
```
kubectl get pods
```

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





	