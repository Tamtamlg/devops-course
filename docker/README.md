# Docker secrets

1. Initialize a swarm

```bash
docker swarm init
```

2. Put `db_password.txt` and `db_root_password.txt` with passwords to 'secrets' directory

3. Deploy stack

```bash
docker stack deploy -c docker-compose.yml wp
```