# slurm-cluster
Docker local SLURM cluster

Created by: Rodrigo Ancavil del Pino

https://medium.com/analytics-vidhya/slurm-cluster-with-docker-9f242deee601

To build the required images for the SLUM cluster:

```shell
$ docker compose build
```

To run the SLURM cluster environment:

```shell
 $ docker compose up -d
```

To stop it:

```shell
$ dockøer compose stop
```

To restart it:

```shell
$ docker compose start
```

To check logs:

```shell
$ docker compose logs -f
```

To check running containers:

```shell
$ docker compose ps
```

To destroy the SLURM culster completely:
```shell
$ docker compsoe down -v
```
