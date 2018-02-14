## Description

Docker-based CI service setup using Jenkins, SonarQube and Traefik.

Service provides
1. [Preconfigured Jenkins instance][jenkins]
2. [Preconfigured SonarQube instance][sonarqube]
3. Reverse proxy for Jenkins and SonarQube with Let's Encrypt support using [Traefik][traefik]

## Deployment
To deploy using [docker-machine][docker-machine]

1. Create docker-machine config using preferred cloud provider.
For example, to use existing server with ssh connection execute
```
docker-machine create \
    --driver generic \
    --generic-ip-address=xx.xx.xx.xx \
    --generic-ssh-key ~/.ssh/id_rsa \
    --generic-ssh-user=ubuntu \
    ciserver
```

2. Copy configs and secrets to remote machine
```
docker-machine scp jenkins.yml ubuntu@ciserver:/home/ubuntu
docker-machine scp acme.json ubuntu@ciserver:/home/ubuntu
docker-machine scp traefik.toml ubuntu@ciserver:/home/ubuntu
docker-machine scp --recursive secrets ubuntu@ciserver:/home/ubuntu
```

3. Setup environment variables to connect to remote Docker daemon

```
eval $(docker-machine env ciserver)
```

4. Launch service on remote machine using [docker-compose][docker-compose]
```
docker-compose pull
WORKDIR=/home/ubuntu JENKINS_HOST=jenkins.example.com SONARQUBE_HOST=sonarqube.example.com docker-compose up
```

5. Reset environment varibles
```
eval $(docker-machine env --unset)
```


[traefik]: https://github.com/containous/traefik
[jenkins]: https://github.com/alapshin/jenkins-master
[sonarqube]: https://github.com/alapshin/docker-sonarqube
[docker-machine]: https://docs.docker.com/machine/
[docker-compose]: https://docs.docker.com/compose/
