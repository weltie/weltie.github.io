# Empty MD Topic

![image_23.png](image_23.png)Start typing here...

分布式JOIN
# shuffel join

# boardcast join

# colocate join


docker run \
--rm \
-e SONAR_HOST_URL=http://172.17.0.3:9000/  \
-e SONAR_TOKEN=sqp_619d5db23e2aff0755a4dfeffca64affeedea698  \
-e SONAR_PROJECT_KEY=openbi_flow-server_5d227b14-6d86-4e0d-a9ca-7ca701a02e02  \
-v "/Users/lem/Work/GitLab/flow-server:/usr/src" \
sonarsource/sonar-scanner-cli

docker run --rm \
    -v "/Users/lem/Work/GitLab/flow-server:/usr/src" \
    sonarsource/sonar-scanner-cli \
    -Dsonar.projectKey=openbi_flow-server_5d227b14-6d86-4e0d-a9ca-7ca701a02e02 \
    -Dsonar.host.url=http://172.17.0.3:9000 \
    -Dsonar.token=sqp_619d5db23e2aff0755a4dfeffca64affeedea698


docker buildx build --platform linux/amd64,linux/arm64 -t tca-client .

docker run \
--rm \
-e SONAR_HOST_URL=http://172.17.0.4:9000/  \
-e SONAR_TOKEN=sqp_619d5db23e2aff0755a4dfeffca64affeedea698  \
-e SONAR_PROJECT_KEY=openbi_flow-server_5d227b14-6d86-4e0d-a9ca-7ca701a02e02  \
-v "/Users/lem/Work/GitLab/flow-server:/usr/src" \
sonarsource/sonar-scanner-cli -help

sonar-scanner \
  -Dsonar.projectKey=openbi_flow-server_5d227b14-6d86-4e0d-a9ca-7ca701a02e02 \
  -Dsonar.sources=/Users/lem/Work/GitLab/flow-server \
  -Dsonar.host.url=http://172.17.0.3:9000 \
  -Dsonar.token=sqp_2d42a760a669b911666e395c8ddb22f7388c879c

docker run -d --name dpanel --restart=always \
 -p 9080:80 -p 9443:443 -p 9807:8080 \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -v /Users/lem/Home/dpanel:/dpanel -e APP_NAME=dpanel dpanel/dpanel:latest