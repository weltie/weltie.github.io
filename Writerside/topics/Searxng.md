# Searxng


```Shell
docker pull searxng/searxng

docker run --rm -d -p 8080:8080 \
             -v "${PWD}/:/etc/searxng" \
             -e "BASE_URL=http://localhost:8080/" \
             -e "INSTANCE_NAME=my-searxng" \
             searxng/searxng
```
