# Docker secrets

1. Initialize a swarm

```bash
docker swarm init
```

2. Create secrets `db_password` and `db_root_password`:
```bash
printf "some password for db user" | docker secret create db_password -
```
```bash
printf "some password for db root" | docker secret create db_root_password -
```

3. Deploy stack

```bash
docker stack deploy -c docker-compose.yml wp
```