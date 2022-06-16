# Docker Registry Static as standalone example

You can set up the static user interface as standalone in several ways.

If you want to populate your registry, use `populate.sh` script.
The interface will be accessible with <http://localhost>.
Your docker registry will be accessible with <http://localhost:5000>.

If you want to access from anywhere, edit "./registry-config/simple.yml" as
  Access-Control-Allow-Origin: ['http://localhost']  ->  Access-Control-Allow-Origin: ['*']

1. Use Docker Pull
docker run \
  -p 5000:5000 \
  -v ./registry-data:/var/lib/registry \
  -v ./registry-config/simple.yml:/etc/docker/registry/config.yml
  registry:2.7
docker run \
  -p 80:80 \
  -e REGISTRY_TITLE=My Private Docker Registry \
  -e REGISTRY_URL=http://localhost:5000 \
  -e SINGLE_REGISTRY=true \
  -e DELETE_IMAGES=true \
  joxit/docker-registry-ui:latest

2. Use Docker-Compose
The simplest way is with `simple.yml` docker-compose file.

```sh
docker-compose -f simple.yml up -d
./populate.sh
```

You can add some credentials to access your registry with `credentials.yml` docker-compose file.
Credentials for this example are login: `registry` and password: `ui` using bcrypt.

```sh
docker-compose -f credentials.yml up -d
./populate.sh
```
