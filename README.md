# srkdocker
## Docker Commands
```console
export PS1=" "
docker pull busybox
docker run busybox echo "hello from busybox"
docker ps -a
docker run -it busybox sh
docker rm $(docker ps -a -q -f status=exited)
```
Quoting text with markdown
> This is an example of quoting text with markdown.
This is an example of quoting code
```
echo helloworld
```

[Docker Command Ref.](https://docker-curriculum.com)



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





