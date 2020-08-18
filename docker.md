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
