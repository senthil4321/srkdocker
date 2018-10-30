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
```
https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

[Docker Command Ref.](https://docker-curriculum.com)
[Shell Command Shortcuts](https://stackoverflow.com/questions/9679776/how-do-i-clear-delete-the-current-line-in-terminal)


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





