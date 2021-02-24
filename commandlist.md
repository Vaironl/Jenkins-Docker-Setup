# If you have not created the network

```
docker network create jenkins
```

<br>

# Run docker in docker container. I have removed --rm option so I can easily start the container in the future

```
docker run --name jenkins-docker --detach ^
--privileged --network jenkins --network-alias docker ^
--env DOCKER_TLS_CERTDIR=/certs ^
--volume jenkins-docker-certs:/certs/client ^
--volume jenkins-data:/var/jenkins_home ^
docker:dind
```

<br>


# Create the actual Jenkins container. Also removed the --rm option here.

```
docker run --name jenkins-blueocean --detach ^
--network jenkins --env DOCKER_HOST=tcp://docker:2376 ^
--env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 ^
--volume jenkins-data:/var/jenkins_home ^
--volume jenkins-docker-certs:/certs/client:ro ^
--publish 8080:8080 --publish 50000:50000 jenkins-blueocean:1.1
```