# AERPAW Operator CI/CD

### **THIS IS A WORK IN PROGRESS**

TODO:

- improve deployment docs
- merge info from [https://github.com/mjstealey/jenkins-nginx-docker]() regarding new debian:buster based build
- better link environment file to deployment parameters (presently a mix of env and manual config)

## Configuration

**NOTE**: Instructions defined herein are based on the deployment that resides at [https://aerpaw-ci.renci.org/](). It should be understood that this is a representative deployment, and that parameter changes would be required for deployment into other hosts.

Create a service level user for deploying the applications and give them docker rights

```console
$ id jenkins
uid=995(jenkins) gid=990(jenkins) groups=990(jenkins),991(docker)
```

Ensure a valid SSL certificate is located somewhere on the host for the docker containers to use

```console
# ls -alh /root/cert/
-rw-r--r--  1 root root 2.4K Jun 17  2020 star_renci_org.crt
-rw-r--r--  1 root root 1.7K Jun 17  2020 star_renci_org.key
```

Create a location on the local host to preserve application data

```
/var/aerpaw/
|__ gitea/gitea_data/      # owned by root
|__ jenkins/jenkins_home/  # owned by jenkins
|__ logs/               
    |__ nginx              # owned by root
```

Create a `.env` file and update

```
# jenkins - jenkins.nginx.docker:lts
JENKINS_HOME=/var/aerpaw/jenkins/jenkins_home
UID_JENKINS=995
GID_JENKINS=990
JENKINS_OPTS="--prefix=/jenkins"

# nginx - nginx:latest
NGINX_DEFAULT_CONF=./nginx/default.conf
NGINX_LOGS=./logs/nginx
NGINX_SSL_CERT=/root/cert/star_renci_org.crt
NGINX_SSL_KEY=/root/cert/star_renci_org.key
```

## Deploy

Deploy using Docker Compose

```
docker-compose pull
docker-compose build
docker-compose up -d
```

## Jenkins


## Gitea

Create a Gitea Administrative User

```
export CREATE_USER='gitea admin create-user --name MY_USER --password MY_PASSWORD --email MY_EMAIL admin'
docker exec -u git gitea sh -c "${CREATE_USER}"
```

## Nginx
