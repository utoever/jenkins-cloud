# Jenkins Cloud

+ Write .env

```yml
DOCKER_IMAGE = {__REGISTRY_SERVER__}/jenkins-cloud:latest
JENKINS_SERVER = {__JENKINS_SERVER__}
JENKINS_PASSWD = {__YOUR_PASSWD__}
JENKINS_PID = {__YOUR_PROJECT_ID__}
```

+ Write docker-compose.yml

```yml
version: "3"
services:
  jenkins:
    image: ${DOCKER_IMAGE}
    container_name: jenkins
    user: root
    privileged: true
    environment:
      - JENKINS_SERVER=${JENKINS_SERVER}
      - JENKINS_PASSWD=${JENKINS_PASSWD}
      - JENKINS_PID=${JENKINS_PID}
      - JENKINS_OPTS="--prefix=/jenkins"
      - JAVA_OPTS="-Djava.awt.headless=true"
      - JAVA_OPTS="-Dcom.sun.akuma.Daemon=daemonized"
      # - JAVA_OPTS="-Dcasc.jenkins.config=/var/jenkins_home/jenkins-config.yml"
      - TZ=Asia/Seoul
    volumes:
      # - ./gitconfig:/root/.gitconfig
      # - ./settings.xml:/root/.m2/settings.xml
      - /appdata/jenkins_home:/var/jenkins_home
      - /appdata/m2/repository:/appdata/m2/repository
      - /usr/bin/docker:/bin/docker
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "8080:8080"
      - "50000:50000"
    restart: unless-stopped
    # extra_hosts:
    #   - ns.google.com:8.8.8.8
```

---

+ Run docker

```sh
export GHCR=ghcr.io/utoever
docker pull $GHCR/jenkins-cloud:latest
```
