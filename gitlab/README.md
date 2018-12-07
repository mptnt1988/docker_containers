Setup Nginx for GitLab first: see nginx & ssl directories.
```
git clone git@bitbucket.org:mptnt1988/docker_containers.git
```
```
cd docker_containers/
```
```
docker run -d --hostname gitlab.local.tech --name gitlab -p 127.0.0.1:10080:443 -p 1122:22 -p 5005:5005 -v /srv/docker/gitlab/gitlab_config:/etc/gitlab -v /srv/docker/gitlab/gitlab_logs:/var/log/gitlab -v /srv/docker/gitlab/gitlab_data:/var/opt/gitlab --env-file gitlab/docker.env --restart always gitlab/gitlab-ce:11.5.3-ce.0
```
Wait until gitlab container status is healthy... Then:
```
docker cp ssl/ca.crt gitlab:/usr/local/share/ca-certificates/ca.crt
docker exec -it gitlab update-ca-certificates
docker cp ssl/ca.key gitlab:/etc/gitlab/ssl/gitlab.local.tech.key
docker cp ssl/ca.crt gitlab:/etc/gitlab/ssl/gitlab.local.tech.crt
docker exec -it gitlab gitlab-ctl restart nginx
```
