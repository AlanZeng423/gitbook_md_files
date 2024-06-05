# Docker Commands

## Create Docker Image

配置依赖

```bash
pip freeze > requirements.txt
touch Dockerfile
vim Dockerfile


```

edit Dockerfile

```docker
#基于的基础镜像
FROM python:3.10.12
#代码添加到code文件夹
ADD . /code
# 设置code文件夹是工作目录
WORKDIR /code
# 安装支持
RUN pip install -r requirements.txt
CMD ["python", "/hello.py"]
```

build docker image & run

```bash
docker build -t demo .
docker run -it demo
docker run -itd --name demo -p 5000:5000 demo
```

***

## Docker Image  Push\&Pull

{% embed url="https://hub.docker.com/" %}
Docker Hub
{% endembed %}

### login to Docker Hub&#x20;

```bash
[root@localhost ~]#  docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username:          ##输入账号
Password:          ##输入密码
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```



### Edit Tag

* Push Command `docker push docker_username/REPOSITORY:TAG`
* We need to edit the tag and repository name:

```bash
[root@localhost ~]# docker images 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               stable-perl         9e5f6711d527        3 days ago          178MB
nginx               v1                  9e5f6711d527        3 days ago          178MB
[root@localhost ~]# docker tag nginx:v1 llxxyy/nginx-io:v1
[root@localhost ~]# docker images 
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               stable-perl         9e5f6711d527        3 days ago          178MB
nginx               v1                  9e5f6711d527        3 days ago          178MB
llxxyy/nginx-io     v1                  9e5f6711d527        3 days ago          178MB
[root@localhost ~]# 

```

### Push

```bash
[root@localhost ~]# docker push llxxyy/nginx-io:v1
The push refers to repository [docker.io/llxxyy/nginx-io]
833a0f6a6ff9: Pushed 
10bfe402500e: Pushed 
d43641d7d594: Mounted from library/nginx 
c2adabaecedb: Mounted from library/nginx 
v1: digest: sha256:67dcdae5578c0374019cc899731543cfd7c48fe5780e84233a258f2bf7d2ceda size: 1155
[root@localhost ~]# 
```



### Pull

```
[root@localhost ~]# docker pull llxxyy/nginx-io:v1
v1: Pulling from llxxyy/nginx-io
Digest: sha256:67dcdae5578c0374019cc899731543cfd7c48fe5780e84233a258f2bf7d2ceda
Status: Downloaded newer image for llxxyy/nginx-io:v1
docker.io/llxxyy/nginx-io:v1
```



```bash
docker ps -a
```

##

