
---
> **NOTE** Official flink docker images are built from here https://github.com/apache/flink-docker. 
> The docker image built here is custom for experimentation purposes.

> This image also picks up binaries  from here https://repo.maven.apache.org/maven2/org/apache/flink/  
---

# How to create the docker image

```shell script
cd 1.12/scala_2.11-java8-debian/

NAME=flink-image
TAG=1.12.0.1

SOURCE_IMG=$NAME:$TAG
/usr/local/bin/docker build -t $SOURCE_IMG .

SHA256=$(/usr/local/bin/docker inspect --format='{{index .Id}}' $SOURCE_IMG)
IFS=':' read -ra TOKENS <<< "$SHA256"

echo "IFS:$IFS"

export CR_PAT=a58bc801b78791c95df837e3401e1394d7c6ee43
echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin
docker tag flink-image:$TAG ghcr.io/srinipunuru/flink-image:$TAG
docker push ghcr.io/srinipunuru/flink-image:$TAG
```

# Testing the container

- Comment out the Entrypoint from the dockerfile
- Build the docker image 
- Run the docker image using the below command

```shell
docker run -it flink-image:1.12.0.1 bash
```