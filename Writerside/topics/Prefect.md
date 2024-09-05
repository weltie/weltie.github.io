# Prefect

# postgres
```
docker run -d --name prefect-postgres \
    -v ./prefectdb:/var/lib/postgresql/data \
    -p 5432:5432 \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=yourTopSecretPassword \
    -e POSTGRES_DB=prefect \
    postgres:latest
```