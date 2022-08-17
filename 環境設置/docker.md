# docker

## docker-compose

建立container網路關係圖

## image

## start container

`docker start {container name}`

## build container

`docker run {image}`
`docker run ubuntu`
`docker run --name={container name} {image name}`
`docker run --name=ubuntuTest ubuntu`

## file sharing

`docker run -v {loacl path}:{docker path} {build c}`
`docker run -v ~/User:/User --name=ubuntuTest ubuntu`

## dockerfile

- FROM
  
  `FROM {image}` or `FROM {image}:{tag}`

  ex : `FROM ubuntu`

- MAINTAINER
  
  `MAINTAINER {name}`

  ex : `MAINTAINER jand`

- RUN
  
  `RUN {command}`
  ex : `RUN apt update`

- EXPOSE
  
  `EXPOSE`

```text
FROM ubuntu
MAINTAINER jand

RUN apt update
RUN apt install -y python3-pip
RUN pip3 install -r requirements.txt

EXPOSE 5000
```

### build

`docker build -t mytomcat . --no-cache`

## python

- python3.6
- pip

docker exec -it 5378ef9bcd6f bash

apt update

apt install -y python3-pip

---
pip3 install pipenv

or

git:
pip freeze > requirements.txt

pip3 install -r requirements.txt

---

## 更換 docker image container 存放目錄

---

1. Stop the docker daemon

```cmd
sudo service docker stop
```

2. Add a configuration file to tell the docker daemon what is the location of the data directory

Using your preferred text editor add a file named daemon.json under the directory /etc/docker. The file should have this content:

```cmd
{ 
   "data-root": "/path/to/your/docker" 
}
```

3. Copy the current data directory to the new one

```cmd
sudo rsync -aP /var/lib/docker/ /path/to/your/docker
```

4. Rename the old docker directory

```cmd
sudo mv /var/lib/docker /var/lib/docker.old
```

5. Restart the docker daemon

```cmd
sudo service docker start
```

6. Test

```cmd
sudo rm -rf /var/lib/docker.old
```

### 疑難雜症

dodolab S223
[Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?](https://blog.yowko.com/cannot-connect-docker-daemon/)
