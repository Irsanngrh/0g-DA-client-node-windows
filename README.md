<h2 align=center>Instalasi 0G DA Client Node menggunakan windows 10/11 ( tanpa vps )</h2>

## Instalasi Awal
- Install [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/) dan [Ubuntu terminal](https://apps.microsoft.com/detail/9pdxgncfsczv?rtc=1&hl=id-ID&gl=ID)
- Buka Ubuntu terminal, lalu ketik kode berikut:
```
git clone https://github.com/0glabs/0g-da-client.git
```
- Buat docker image:
```
cd 0g-da-client
docker build -t 0g-da-client -f combined.Dockerfile .
```
- Buat file env:
```
nano envfile.env
```
- Copy kode berikut dan paste pada file env (hanya ubah pada bagian YOUR_PRIVATE_KEY):
```
# envfile.env
COMBINED_SERVER_CHAIN_RPC=https://evmrpc-testnet.0g.ai
COMBINED_SERVER_PRIVATE_KEY=YOUR_PRIVATE_KEY
ENTRANCE_CONTRACT_ADDR=0x857C0A28A8634614BB2C96039Cf4a20AFF709Aa9

COMBINED_SERVER_RECEIPT_POLLING_ROUNDS=180
COMBINED_SERVER_RECEIPT_POLLING_INTERVAL=1s
COMBINED_SERVER_TX_GAS_LIMIT=2000000
COMBINED_SERVER_USE_MEMORY_DB=true
COMBINED_SERVER_KV_DB_PATH=/runtime/
COMBINED_SERVER_TimeToExpire=2592000
DISPERSER_SERVER_GRPC_PORT=51001
BATCHER_DASIGNERS_CONTRACT_ADDRESS=0x0000000000000000000000000000000000001000
BATCHER_FINALIZER_INTERVAL=20s
BATCHER_CONFIRMER_NUM=3
BATCHER_MAX_NUM_RETRIES_PER_BLOB=3
BATCHER_FINALIZED_BLOCK_COUNT=50
BATCHER_BATCH_SIZE_LIMIT=500
BATCHER_ENCODING_INTERVAL=3s
BATCHER_ENCODING_REQUEST_QUEUE_SIZE=1
BATCHER_PULL_INTERVAL=10s
BATCHER_SIGNING_INTERVAL=3s
BATCHER_SIGNED_PULL_INTERVAL=20s
BATCHER_EXPIRATION_POLL_INTERVAL=3600
BATCHER_ENCODER_ADDRESS=DA_ENCODER_SERVER
BATCHER_ENCODING_TIMEOUT=300s
BATCHER_SIGNING_TIMEOUT=60s
BATCHER_CHAIN_READ_TIMEOUT=12s
BATCHER_CHAIN_WRITE_TIMEOUT=13s
```

## Intalasi Akhir
CARA 1 :
- Untuk menjalankan node, ketik kode berikut:
```
docker run -d --env-file envfile.env --name 0g-da-client -v ./run:/runtime -p 51001:51001 0g-da-client combined 
```
- Untuk cek status node, ketik kode berikut:
```
docker logs -f CONTAINER_ID
```
- Untuk menhentikan node, ketik kode berikut:
```
docker stop CONTAINER_ID
```

CARA 2 :
- Untuk menjalankan ulang node, ketik kode berikut:
```
cd 0g-da-client
docker run -v ./run:/runtime -p 51001:51001 0g-da-client:latest combined \
    --chain.rpc https://evmrpc-testnet.0g.ai \
    --chain.private-key YOUR_PRIVATE_KEY \
    --chain.receipt-wait-rounds 180 \
    --chain.receipt-wait-interval 1s \
    --chain.gas-limit 2000000 \
    --combined-server.use-memory-db \
    --combined-server.storage.kv-db-path /runtime/ \
    --combined-server.storage.time-to-expire 2592000 \
    --disperser-server.grpc-port 51001 \
    --batcher.da-entrance-contract ENTRANCE_CONTRACT_ADDR \
    --batcher.da-signers-contract 0x0000000000000000000000000000000000001000 \
    --batcher.finalizer-interval 20s \
    --batcher.confirmer-num 3 \
    --batcher.max-num-retries-for-sign 3 \
    --batcher.finalized-block-count 50 \
    --batcher.batch-size-limit 500 \
    --batcher.encoding-interval 3s \
    --batcher.encoding-request-queue-size 1 \
    --batcher.pull-interval 10s \
    --batcher.signing-interval 3s \
    --batcher.signed-pull-interval 20s \
    --batcher.expiration-poll-interval 3600 \
    --encoder-socket DA_ENCODER_SERVER \
    --encoding-timeout 300s \
    --signing-timeout 60s \
    --chain-read-timeout 12s \
    --chain-write-timeout 13s
```
