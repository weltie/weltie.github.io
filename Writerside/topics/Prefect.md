# Prefect

## docker install
### postgres
```
docker run -d --name prefect-postgres \
    -v ./prefectdb:/var/lib/postgresql/data \
    -p 5432:5432 \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=yourTopSecretPassword \
    -e POSTGRES_DB=prefect \
    postgres:latest
```


docker run -d --name prefect-postgres -v prefectdb:/var/lib/postgresql/data -p 9009:5432 -e POSTGRES_USER=prefect -e POSTGRES_PASSWORD=prefect -e POSTGRES_DB=prefect postgres:latest

prefect config set PREFECT_API_URL="http://127.0.0.1:9006/api"
prefect config set PREFECT_API_DATABASE_CONNECTION_URL="postgresql+asyncpg://prefect:prefect@127.0.0.1:9009/prefect"
prefect config set PREFECT_WORKER_QUERY_SECONDS=5
prefect config view

nohup ./server.sh > server.log 2>&1 &

## add custom block
`prefect block register --file /path/.../blocks.py`