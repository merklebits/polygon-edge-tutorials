# Setting up a local Polygon Edge validator for a remote Bootnode.

This tutorial will have 2 steps:
- Setting up the remote bootnode on AWS
- Connecting a local validator to the local bootnode

## 1. Setting up a Remote Bootnode on AWS

### Setting up Polygon Edge

Set up a t3.large with Ubuntu 22.04 LTS. Follow the instructions here till Step 2. For Step 3 there will be slight changes where we edit the Docker config to expose the p2p ports of the bootnodes.

### Exposing P2P port from Docker

If we check the `docker-compose`
```
cd polygon-edge/docker/local
cat docker-compose.yml
``` 
We will see that the p2p ports exposed on port 1478 within the container is not mapped to a port on the host. We will have to add that mapping manually.
```
sudo vim docker-compose.yml
```
Add the following line to the file
```
  node-1:
    build:
      context: ../../
      dockerfile: docker/local/Dockerfile
    command: ["server", "--data-dir", "/data", "--chain", "/genesis/genesis.json", "--grpc-address", "0.0.0.0:9632", "--libp2p", "0.0.0.0:1478", "--jsonrpc", "0.0.0.0:8545", "--prometheus", "0.0.0.0:5001", "--seal"]
    depends_on:
      init:
        condition: service_completed_successfully
    ports:
      - '10000:9632'
      - '10001:1478' #Add this line
      - '10002:8545'
      - '10003:5001'
    volumes:
      - node-1:/data
      - genesis:/genesis
    networks:
      - polygon-edge-docker
    restart: on-failure
```
### Building and running the containers.

```
sudo docker-compose build
sudo docker-compose up -d
```

## 2. Setting up a Remote Bootnode on AWS

