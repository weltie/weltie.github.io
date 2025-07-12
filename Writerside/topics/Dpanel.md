# Dpanel

docker run -d --name dpanel --restart=always \
 -p 9017:8080 -e APP_NAME=dpanel \
 -v /var/run/docker.sock:/var/run/docker.sock -v dpanel:/dpanel \
 dpanel/dpanel:lite


docker run -d --name dpanel --restart=always \
 -p 9017:8080 -e APP_NAME=dpanel \
 -v /var/run/docker.sock:/var/run/docker.sock -v dpanel:/dpanel \
shanghai-tongyong.tencentcloudcr.com/base/dpanel:lite


docker run -d --name dpanel --restart=always \
 -p 8807:8080 -e APP_NAME=dpanel \
 -v /Users/lem/.docker/run/docker.sock:/var/run/docker.sock \
 -v .:/dpanel dpanel/dpanel:lite

docker run -d --name sonarqube-local \
    -p 9011:9000 \
    -e SONAR_JDBC_URL=jdbc:postgresql://172.17.0.3:5432/ \
    -e SONAR_JDBC_USERNAME=postgres \
    -e SONAR_JDBC_PASSWORD=postgres \
    -v ./data:/opt/sonarqube/data \
    -v ./extensions:/opt/sonarqube/extensions \
    -v ./logs:/opt/sonarqube/logs \
    sonarqube:latest

docker run --rm \
    -v "/Users/lem/Work/GitLab/flow-server:/usr/src" \
    sonarsource/sonar-scanner-cli \
    -Dsonar.projectKey=FLOWING \
    -Dsonar.host.url=http://172.17.0.5:9000 \
    -Dsonar.token=sqp_0d97f5ed01636fc9e03174ba59c5253f7df1229e

docker run --rm \
    -v "/Users/lem/Work/GitLab/flow-server:/usr/src" \
    sonarsource/sonar-scanner-cli \
    -Dsonar.projectKey=openbi_flow-server_5d227b14-6d86-4e0d-a9ca-7ca701a02e02 \
    -Dsonar.host.url=http://172.17.0.3:9000 \
    -Dsonar.token=sqp_0d97f5ed01636fc9e03174ba59c5253f7df1229e

docker run -it --rm  -v $PWD:/workspace/client -v /Users/lem/Work/GitLab/flow-server:/workspace/src  --name tca-client tca-client
