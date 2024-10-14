# Glide ETL 

Glide ETL lets you convert blockchain data into convenient formats like CSVs and relational databases.

[Example Public Data Set BigQuery](https://console.cloud.google.com/marketplace/details/ethereum/crypto-ethereum-blockchain).


## Quickstart

Install Glide ETL:

```bash
pip3 install glide-etl
```

Export blocks and transactions ([Schema](docs/schema.md#blockscsv), [Reference](docs/commands.md#export_blocks_and_transactions)):

```bash
> glideetl export_blocks_and_transactions --start-block 0 --end-block 500000 \
--blocks-output blocks.csv --transactions-output transactions.csv \
--provider-uri https://mainnet.infura.io/v3/7aef3f0cd1f64408b163814b22cc643c
```

Export ERC20 and ERC721 transfers ([Schema](docs/schema.md#token_transferscsv), [Reference](docs/commands.md##export_token_transfers)):

```bash
> glideetl export_token_transfers --start-block 0 --end-block 500000 \
--provider-uri file://$HOME/Library/Glide/geth.ipc --output token_transfers.csv
```

Export traces ([Schema](docs/schema.md#tracescsv), [Reference](docs/commands.md#export_traces)):

```bash
> glideetl export_traces --start-block 0 --end-block 500000 \
--provider-uri file://$HOME/Library/Glide/parity.ipc --output traces.csv
```

---

Stream blocks, transactions, logs, token_transfers continually to console ([Reference](docs/commands.md#stream)):

```bash
> pip3 install glide-etl[streaming]
> glideetl stream --start-block 500000 -e block,transaction,log,token_transfer --log-file log.txt \
--provider-uri https://mainnet.infura.io/v3/7aef3f0cd1f64408b163814b22cc643c
```

For the latest version, check out the repo and call 
```bash
> pip3 install -e . 
> python3 glideetl.py
```

## Running Tests

```bash
> pip3 install -e .[dev,streaming]
> export ETHEREUM_ETL_RUN_SLOW_TESTS=True
> export PROVIDER_URL=<your_porvider_uri>
> pytest -vv
``` 

### Running Tox Tests

```bash
> pip3 install tox
> tox
```

## Running in Docker

0. Glide-ETL Available at Hub Docker :
        
        > docker pull avenbreak/glide-etl:latest

1. Install Docker: https://docs.docker.com/get-docker/

2. Build a docker image
        
        > docker build -t glide-etl:latest .
        > docker image ls
        
3. Run a container out of the image

        > docker run -v $HOME/output:/glide-etl/output glide-etl:latest export_all -s 0 -e 5499999 -b 100000 -p https://mainnet.infura.io
        > docker run -v $HOME/output:/glide-etl/output glide-etl:latest export_all -s 2018-01-01 -e 2018-01-01 -p https://mainnet.infura.io

4. Run streaming to console or Pub/Sub

        > docker build -t glide-etl:latest .
        > echo "Stream to console"
        > docker run glide-etl:latest stream --start-block 500000 --log-file log.txt
        > echo "Stream to Pub/Sub"
        > docker run -v /path_to_credentials_file/:/glide-etl/ --env GOOGLE_APPLICATION_CREDENTIALS=/glide-etl/credentials_file.json glide-etl:latest stream --start-block 500000 --output projects/<your-project>/topics/crypto_ethereum

If running on an Apple M1 chip add the `--platform linux/x86_64` option to the `build` and `run` commands e.g.:

```
docker build --platform linux/x86_64 -t glide-etl:latest .
docker run --platform linux/x86_64 glide-etl:latest stream --start-block 500000
```
