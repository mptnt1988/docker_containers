Setup nginx for Jenkins first: see nginx & ssl directories.
```
git clone git@bitbucket.org:mptnt1988/docker_containers.git
```
```
cd docker_containers/
```
```
docker build -t jenkins -f jenkins/Dockerfile --no-cache .
```
```
docker run -d --hostname jenkins.local.tech --name jenkins -p 127.0.0.1:10180:8080 -p 50000:50000 -v /srv/docker/jenkins/jenkins_home:/var/jenkins_home --restart always jenkins
```
