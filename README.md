docker-mvnd
============

## Building local Docker image

```shell
# build 
cd eclipse-temurin-11 
docker build -t mvnd:3.9-eclipse-temurin-11 .

cd eclipse-temurin-11-alpine
docker build -t mvnd:3.9-eclipse-temurin-11-alpine .

# use
docker run -it -v "$(pwd)":/project -w /project mvnd:3.9-eclipse-temurin-11 mvn verify

```

## reference:

- [docker-maven](https://github.com/carlossg/docker-maven)
- [maven-mvnd](https://github.com/apache/maven-mvnd)