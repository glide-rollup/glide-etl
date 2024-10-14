# Uploading to Docker Hub

```bash
glideetl_VERSION=1.11.0
docker build -t ethereum-etl:${glideetl_VERSION} -f Dockerfile .
docker tag ethereum-etl:${glideetl_VERSION} blockchainetl/ethereum-etl:${glideetl_VERSION}
docker push blockchainetl/ethereum-etl:${glideetl_VERSION}

docker tag ethereum-etl:${glideetl_VERSION} blockchainetl/ethereum-etl:latest
docker push blockchainetl/ethereum-etl:latest
```