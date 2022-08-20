# Jenkins

## Install docker and docker-compose

https://docs.docker.com/compose/install/

### Docker

```
sudo apt-get update
```

```
sudo apt install docker.io
```

### Docker-compose

{% code overflow="wrap" %}
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
{% endcode %}

```
sudo chmod +x /usr/local/bin/docker-compose
```

#### Create the docker group if it does not exist

```
sudo groupadd docker
```

#### Add your user to the docker group.

```
sudo usermod -aG docker $USER
```

```
newgrp docker
```

## **Install portainer**

{% code overflow="wrap" %}
```
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```
{% endcode %}

## **Install Jenkins**

https://hub.docker.com/\_/jenkins?tab=description\&page=1\&ordering=last\_updated

{% code overflow="wrap" %}
```
docker run -d --restart=always -p 8080:8080 --privileged -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):$(which docker) -v jenkins_home:/var/jenkins_home -v  /usr/local/bin/docker-compose:/usr/local/bin/docker-compose --user root:root --name jenkins-server4 jenkins/jenkins:lts
```
{% endcode %}

### install by docker-compose

{% code lineNumbers="true" %}
```yaml
version: '3.7'
services:
  jenkins:
    image: jenkins/jenkins:lts
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - /jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/bin/docker:/usr/bin/docker
      - /usr/local/bin/docker-compose:/usr/local/bin/docker-compose
```
{% endcode %}

***

## Hướng dẫn setup jenkins

* Get key

![2022-07-02\_185607](https://user-images.githubusercontent.com/55792941/176999740-59ab49ac-e1c8-4090-8f46-fc8ab82b0148.jpg)

* Tạo new project

![2022-07-02\_191743](https://user-images.githubusercontent.com/55792941/177000487-eed1059d-2365-4f38-b82d-b70f869f097d.jpg)

* Setup project url

![2022-07-02\_191812](https://user-images.githubusercontent.com/55792941/177000492-20dcd479-80fb-4e4d-911a-165ac7546c52.jpg)

* Setup build triggers

![2022-07-02\_191826](https://user-images.githubusercontent.com/55792941/177000500-1b587736-c65a-4479-aed0-28363908da8d.jpg)

* Setup pipeline

![2022-07-02\_192453](https://user-images.githubusercontent.com/55792941/177000699-7e2ee42a-0747-4825-9cb4-667aff298eec.jpg)

![2022-07-02\_192506](https://user-images.githubusercontent.com/55792941/177000703-1e4c059b-0058-414d-81fc-833734b50018.jpg)

* Hoàn thành

![2022-07-02\_192633](https://user-images.githubusercontent.com/55792941/177000752-fca040d8-56a1-4996-8f09-e02546cb95b1.jpg)

***

## Setup webhook github

* Vào settings -> webhook -> Add webhook
* Chọn Let me select individual events -> Chọn push và pull requests&#x20;

> ![2022-07-02\_192744](https://user-images.githubusercontent.com/55792941/177000854-f0b6f8c6-1d8f-490c-b42d-d3d09d3f6724.jpg)

![2022-07-02\_192938](https://user-images.githubusercontent.com/55792941/177000859-d3100915-b17c-486e-9853-4c5e8fca81fe.jpg)

{% hint style="info" %}


Lưu ý url là url của **jenkins + /github-webhook/**&#x20;

VD: **https://8080-tuanvuprese-cicdjenkins-psx08fn83vn.ws-us47.gitpod.io/github-webhook/**
{% endhint %}
