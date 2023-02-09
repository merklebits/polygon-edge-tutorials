# Setting Up Edge on AWS with docker-compose

For this tutorial we use a `t3.large` instance with `Ubuntu 20.04 `

## 1. Logging on to server
We use `SSH` to log onto our EC2 instance.
```
ssh -i <your_server_key.pem> ubuntu@ec2-<ip>.compute-<X>.amazonaws.com
```
# 2. Install Docker
Follow the instructions [here](https://docs.docker.com/engine/install/ubuntu/) to install docker on Ubuntu.

Next, we install docker-compose
```
sudo apt install python3-pip
sudo pip install docker-compose
```
## 2. Setting up Edge
Clone Edge and switch to the desired release commit. In  this tutorial we will use `v0.6.3` of Polygon Edge. 
```
https://github.com/0xPolygon/polygon-edge
```
Check the commit hash of the release you wish to clone and create a new local branch from that commit.
```
cd polygon-edge
git checkout -b v0.6.3 ae795165d46f06cb68ded8c918ecef93d91be53c
```
## 3. Docker Build and Run Containers
Navigate to the `/docker/local/` directory where you can find the `docker-compose.yml` file.
```
cd docker/local
```
Next, we use `docker-compose` to build the containers. This step can take a while so please be patient.
```
sudo docker-compose build
```
Finally, we run the containers in a detached mode
```
sudo docker-compose up -d
```
To view the list of running containers
```
sudo docker ps -a
```
To bring the containers down
```
sudo docker-compose down
```
## 3. Setting up nginx and certbot

Follow the instructions [here](https://github.com/nonceblox/polygon-edge-tutorials/blob/master/setup_nginx.md) to expose your JSON RPC and Websocket RPC urls to the public via nginx.