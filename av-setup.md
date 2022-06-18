
### reference
- macos - [colima](https://github.com/abiosoft/colima) | [setup](https://smallsharpsoftwaretools.com/tutorials/use-colima-to-run-docker-containers-on-macos/)
- [setup-aidbox.dev](https://docs.aidbox.app/getting-started/installation/setup-aidbox.dev)

### Application URLs
|                 | url                              | Auth         |
|-----------------|----------------------------------|--------------|
| aidbox          | http://localhost:8888/ui/console | admin/secret |
| es              | http://localhost:9200/           |              |
| kibana          | http://localhost:5601/app/home#/ |              |
| graphana        | http://localhost:3000/login      | admin/admin  |
| prometheus push | http://localhost:9091/#          |              |
|                 | http://localhost:9091/metrics    |              |
| prometheus      | http://localhost:9090/           |              |

# Initial Setup
```bash
git clone https://github.com/Aidbox/devbox.git
cd devbox && cp .env.tpl .env
# add license id & secret
```

### Observations
- work okay out of box with `docker-desktop`
- following changes are made for it to work with [colima](https://github.com/abiosoft/colima)
    - resulting in - uid/gid errors
    - `volume` change is based on [git-issue](https://github.com/timescale/timescaledb-docker/issues/151)

### local changes
- add docker volume for config
    - prometheus-config:
    - grafana-config:
    - pg-data:
- update each section with replacing local dir with docker volume


### Run
```bash
docker volume create --driver local \
--opt type=none \
--opt device=${PWD}/audit/prometheus \
--opt o=bind prometheus-config

docker volume create --driver local \
--opt type=none \
--opt device=${PWD}/audit/grafana/provisioning \
--opt o=bind graphana-config

docker compose up -d
```

### tear down
```bash
docker compose down
docker volume rm `(docker volume ls -qf dangling=true)`
```

### cleanup
```bash

```


### troubleshooting/ commands
```bash
docker volume ls
docker compose down
docker volume prune
docker compose up -d

docker-compose logs -f devbox-db
docker-compose logs -f devbox
docker-compose logs -f pushgateway
docker-compose logs -f prometheus
docker-compose logs -f es
docker-compose logs -f kibana
docker-compose logs -f grafana
docker-compose logs -f apm-server

docker volume rm `(docker volume ls -qf dangling=true)`
```