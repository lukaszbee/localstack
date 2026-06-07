## Localstack wersja w Docker

> W wersji dockerowej mozemy zaoptymaliwoac obraz localstack by mog kozystac w wiekszej ilosci `resourcow`


### Dockerfile dla localstack

```sh
services:
  localstack:
    image: localstack/localstack:latest
    container_name: localstack
    ports:
      - "4566:4566"
    security_opt:
      - "label=disable"
    environment:
      - LOCALSTACK_AUTH_TOKEN=${LOCALSTACK_AUTH_TOKEN}   # ← dodaj
      - SERVICES=lambda,s3,iam,sns,logs,sts,stepfunctions
      - AWS_DEFAULT_REGION=us-east-1
      - DOCKER_HOST=unix:///var/run/docker.sock
      - LAMBDA_RUNTIME_ENVIRONMENT_TIMEOUT=30
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:z"
      - "localstack-data:/var/lib/localstack"

volumes:
  localstack-data:

```

### Toworzenie tokena na profilu localstack

Musimy utworzyc konto na **lokalstack** i utworzyc token potrzebny do komunikacji z serwisem. --> [lockalstack](https://app.localstack.cloud/settings/auth-tokens)
Nastepnie token dodac do zmiennej srodowiskowej `LOCALSTACK_AUTH_TOKEN` ktora dodamy do naszego `/bashrc` lub configuracji `fish` 

```fish
set -gx LOCALSTACK_AUTH_TOKEN "tooo******ken"
```

### Uuchomienie lockalstack z poziomu kontenera

Przechodzimy do foldery gdzie znajduje sie **docker-compose**

wydajemy komendy:

```shell
# uruchomienie 

docker-compose -f localstack-compose.yaml up -d

# zniszczenie lockalstack
docker-compose -f localstack-compose.yaml down
```

następnie idziemy do folderu z projektem i mozemy wydac komendy :

```bash
terraform init
terraform plan
terraform apply
```

Wszystko powinno dzialac !!!
