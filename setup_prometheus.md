# Setting Up Prometheus and Grafana for Polygon Edge

## 1. Setting Prometheus Port
The port to broadcast Prometheus metrics can be specified using the --prometheus flag
```
polygon-edge server --data-dir ./test-chain-1 --chain genesis.json --grpc-address :10000 --libp2p :10001 --jsonrpc :10002 --prometheus :10010 --seal
```
Open your browser on `localhost:10010` to stream Prometheus logs.

## 2. Setting up Grafana
Before setting up Grafana we bring the server up-to-date
```
apt-get update -y
apt-get upgrade -y
```
Download the Grafana GPG key with `wget`, then pipe the output to `gpg`. This will convert the GPG key from base64 to binary format. Then pipe the output to `tee` to store the key in the `/usr/share/keyrings/grafana.gpg` file.
```
wget -q -O - https://packages.grafana.com/gpg.key | gpg --dearmor | sudo tee /usr/share/keyrings/grafana.gpg > /dev/null
```
In this command, the option `-q` turns off the status update message for `wget`, and `-O` outputs the file that you downloaded to the terminal. These two options ensure that only the contents of the downloaded file are pipelined. The `> /dev/null` option will hide the output from your terminal for security reasons.

Next, add the Grafana repository to your APT sources:
```
echo "deb [signed-by=/usr/share/keyrings/grafana.gpg] https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
Refresh your APT cache to update your package lists:
```
sudo apt update
```
You can now install the package
```
sudo apt install grafana
```
Once Grafana is installed, use `systemctl` to start the Grafana server:
```
sudo systemctl start grafana-server
```
Next, verify that Grafana is running by checking the service’s status:
```
sudo systemctl status grafana-server
```
You will receive output similar to this:
```
Output
● grafana-server.service - Grafana instance
     Loaded: loaded (/lib/systemd/system/grafana-server.service; disabled; vendor preset: enabled)
   Active: active (running) since Tue 2022-09-27 14:42:15 UTC; 6s ago
     Docs: http://docs.grafana.org
 Main PID: 4132 (grafana-server)
    Tasks: 7 (limit: 515)
...
```
This output contains information about Grafana’s process, including its status, Main Process Identifier (PID), and more. active (running) shows that the process is running correctly.

Lastly, enable the service to automatically start Grafana on boot:
```
sudo systemctl enable grafana-server
```
You will receive the following output:
```
Output
Synchronizing state of grafana-server.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable grafana-server
Created symlink /etc/systemd/system/multi-user.target.wants/grafana-server.service → /usr/lib/systemd/system/grafana-server.service.
```
## 3. Connecting Prometheus to Grafana
