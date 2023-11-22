# Instructions

## Create certificates
```bash
mkdir ca 
cd ca
openssl req -new -text -out server.req
openssl rsa -in privkey.pem -out server.key
rm privkey.pem
openssl req -x509 -in server.req -text -key server.key -out server.crt
chmod og-rwx server.key
cd ..
```

## Create volumen (not needed at all)
```bash
docker volume create --name=db-data
```

## Build local image
```bash
docker build -t postgres_local .
```

## Execute docker-compose.yml
```bash
docker-compose up -d
```

With these steps we are able to launch a PostgreSQL instance with ssl enabled. In the docker-compose.yml file we can find that the service listens on port 5432 and that the credentials with user and password.