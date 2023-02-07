# Setting Up Edge on AWS EC2

For this tutorial we use a `t3.large` instance with `Ubuntu 20.04 `

## 1. Logging on to server
We use `SSH` to log onto our EC2 instance.
```
ssh -i <your_server_key.pem> ubuntu@ec2-<ip>.compute-<X>.amazonaws.com
```
## 2. Setting up Edge on server
Follow the instructions here to obtain the `genesis.json` file. Use this file to spin Edge nodes on EC2. A minimum of 4 nodes are recommended to preserve IBFT guarantees.
```
polygon-edge server --data-dir ./test-chain-1 --chain genesis.json --grpc-address :10000 --libp2p :10001 --jsonrpc :10002 --seal
```
```
polygon-edge server --data-dir ./test-chain-2 --chain genesis.json --grpc-address :20000 --libp2p :20001 --jsonrpc :20002 --seal
```
```
polygon-edge server --data-dir ./test-chain-3 --chain genesis.json --grpc-address :30000 --libp2p :30001 --jsonrpc :30002 --seal
```
```
polygon-edge server --data-dir ./test-chain-4 --chain genesis.json --grpc-address :40000 --libp2p :40001 --jsonrpc :40002 --seal
```
## 3. Setting up nginx and certbot
We use `nginx` and `certbot` to expose our local JSON-RPC endpoint at `http://localhost:10002` via a domain name like `www.example-rpc.com`.
```
sudo apt install nginx
```
```
sudo apt-get update
sudo apt-get install certbot
sudo apt-get install python3-certbot-nginx
```
Switch to the `/etc/nginx/sites-enabled/` driectory and enter create a file named `www.example-rpc.com.conf` as per your intended domain name.
```
cd /etc/nginx/sites-enabled/
touch www.example-rpc.com.conf
```
Inside the `www.example-rpc.com.conf` file add the following config. Here we expose the port on `localhost:10002`
```
server {
    server_name www.example-rpc.com;
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_pass http://127.0.0.1:10002;
    }
}
```
Save this file, then run this command to verify the syntax of your config and restart `nginx`
```
nginx -t && nginx -s reload
```
## 4. Obtaining SSL/TLS Certificate
```
sudo certbot --nginx -d example.com -d www.example.com
```

If configured correctly you should be able to connect to the JSON RPC at `www.example-rpc.com`