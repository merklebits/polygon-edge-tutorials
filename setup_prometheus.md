# Setting Up Prometheus for Polygon Edge
The port to receive Prometheus metrics can be specified using the --prometheus flag
```
polygon-edge server --data-dir ./test-chain-1 --chain genesis.json --grpc-address :10000 --libp2p :10001 --jsonrpc :10002 --prometheus :10010 --seal
```
Open the browser on `localhost:10010` to stream Prometheus logs.