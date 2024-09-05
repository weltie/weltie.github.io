# Nginx-Docker

### pull image
https://hub.docker.com/r/nginxinc/nginx-unprivileged

```Shell
docker pull nginxinc/nginx-unprivileged
```

### generate conf file
```Shell
cd /your_path/
docker run --rm \ 
    --entrypoint=cat nginx /etc/nginx/nginx.conf > ./nginx.conf
```

### run with your conf file
```Shell
docker run --name my-nginx-container \
    -v ./nginx.conf:/etc/nginx/nginx.conf \
    -v ./charts:/etc/nginx/html/charts \
    -d \
    -p 9091:9091 \
    -p 9092:9092 \
    nginxinc/nginx-unprivileged
```

### example conf
visit http://localhost:9091 to visit /data/www/index.html
connect localhost:9092 to connect 172.17.0.2:6379
```Shell

worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /tmp/nginx.pid;


events {
    worker_connections  1024;
}


http {
    proxy_temp_path /tmp/proxy_temp;
    client_body_temp_path /tmp/client_temp;
    fastcgi_temp_path /tmp/fastcgi_temp;
    uwsgi_temp_path /tmp/uwsgi_temp;
    scgi_temp_path /tmp/scgi_temp;

    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;

    server {
        listen 9091;
        location / {
            root /data/www;
        }

        location /images/ {
            root /data;
        }
    }
}

stream {
    upstream redis {
        server 172.17.0.2:6379;
    }
    server {
        listen 9092;
        proxy_connect_timeout 1s;
        proxy_timeout 3s;
        proxy_pass redis;
    }
}
```