## Description

Docker-based CI service setup using Jenkins, SonarQube and Traefik.

Service provides
1. [Preconfigured Jenkins instance][jenkins]
2. [Preconfigured SonarQube instance][sonarqube]
3. Reverse proxy for Jenkins and SonarQube with Let's Encrypt support using [Traefik][traefik]

## Prerequisites

Machine with installed docker >= 19.03 and ssh connection to said machine

## Deployment
To deploy using docker [context][docker-context]

1. Create remote docker context
```
docker context create ciserver --docker "host=ssh://user@example.com"
```

2. Copy configuration and secrets to remote machine
```
scp jenkins.yml user@example.com:/home/user
scp acme.json user@example.com:/home/user
scp traefik.toml user@example.com:/home/user
scp --recursive secrets ubuntu@example.com:/home/user
```

3. Launch service on remote server using [docker-compose][docker-compose]
```
docker-compose --context ciserver pull
docker-compose --context ciserver up
```


[traefik]: https://github.com/containous/traefik
[jenkins]: https://github.com/alapshin/jenkins-master
[sonarqube]: https://github.com/alapshin/docker-sonarqube
[docker-compose]: https://docs.docker.com/compose/
[docker-context]: https://docs.docker.com/engine/context/working-with-contexts/
