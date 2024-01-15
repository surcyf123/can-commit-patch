# Running subtensor locally

- [Running docker](#running-docker)
- [Compiling your own binary](#compiling-your-own-binary)

## Running docker

### Install git
`sudo apt install git`

### Install Docker Engine
 You can follow Docker's [oficial installation guides](https://docs.docker.com/engine/install/)

### Run node-subtensor container
```bash
git clone https://github.com/opentensor/subtensor.git
cd subtensor
# to run a lite node on the mainnet:
docker compose up -d mainnet-lite # to run a lite node on the mainnet
# or mainnet archive node: docker compose up -d mainnet-archive
# or testnet lite node:    docker compose up -d testnet-lite
# or testnet archive node: docker compose up -d testnet-archive
```

## Compiling your own binary
### Requirements
```bash
sudo apt install build-essential git make clang libssl-dev llvm libudev-dev protobuf-compiler -y
```

### Install Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > rustup-init.sh
chmod +x rustup-init.sh
./rustup-init.sh # you can select default options in the prompts you will be given
source "$HOME/.cargo/env"
```

### Rustup update
```bash
rustup default stable && \
 rustup update && \
 rustup update nightly && \
 rustup target add wasm32-unknown-unknown --toolchain nightly
```

### Compiling
```bash
git clone https://github.com/opentensor/subtensor.git
cd subtensor
cargo build --release --features runtime-benchmarks
```

### Running the node
#### Lite node
```bash
./target/release/node-subtensor \
  --base-path /tmp/blockchain \
  --chain ./raw_spec.json \
  --rpc-external --rpc-cors all \
  --ws-external --no-mdns \
  --ws-max-connections 10000 --in-peers 500 --out-peers 500 \
  --bootnodes /dns/bootnode.finney.opentensor.ai/tcp/30333/ws/p2p/12D3KooWRwbMb85RWnT8DSXSYMWQtuDwh4LJzndoRrTDotTR5gDC \
  --sync warp
``` 

#### Archive node
```bash

./target/release/node-subtensor \
  --base-path /tmp/blockchain \
  --chain ./raw_spec.json \
  --rpc-external --rpc-cors all \
  --ws-external --no-mdns \
  --ws-max-connections 10000 --in-peers 500 --out-peers 500 \
  --bootnodes /dns/bootnode.finney.opentensor.ai/tcp/30333/ws/p2p/12D3KooWRwbMb85RWnT8DSXSYMWQtuDwh4LJzndoRrTDotTR5gDC \
  --pruning=archive
``` 