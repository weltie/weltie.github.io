# GITLAB-RUNNER

docker run -d --name gitlab-runner --restart always \
-v /opt/gitlab-runner/config:/etc/gitlab-runner \
-v /var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:latest

docker run -d --name gitlab-runner --restart always \
-v /var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:latest
