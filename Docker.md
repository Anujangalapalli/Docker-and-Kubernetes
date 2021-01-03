## Docker

Using nginx as example image https://hub.docker.com/_/nginx

## Installing Docker

https://www.docker.com/get-started

### Version Check

docker --version

## Docker images

Download inage

* docker pull nginx

See all images

* docker images

Run image

* docker nginx:latest

Checking running images

* docker container ls | docker pa

Stop image

* docker stop containerID | docker stop containerName

Restart image

* docker start containerId | docker start containerName

Run in detached mode

* docker run -d nginx:latest

Run and map port 8080 to port 80

* docker run -d -p 8080:80 nginx:latest

Multiple ports

* docker run -d -p 8080:80 -p 3000:80 nginx:latest

## Delete containers

View existed containers

* docker ps -a

Delete  Container 

* docker rm containerId | docker containerName

Delete all(only works when none are running)

* docker rm $(docker ps -aq)

Force delete all

* docker rm -f $(docker ps -aq)

## name and Format containers

Name containers

* docker run --name containerName -d -p 8080:80 nginx:latest


