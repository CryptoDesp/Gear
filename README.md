# Gear
Gear Node Installation Instructions </br>
## General information </br>
### [Official documentation](https://wiki.gear-tech.io/docs/node/setting-up)
### System requirements:</br>
CPU: 4 Core </br>
RAM: 8Gb </br>
SSD: 150Gb </br>
OS: Ubuntu 20.04 LTS </br>
# Installing the Nibiru Node </br>
### System update
```
sudo apt update && apt upgrade -y
```
### Download and install the Gear node </br>
copy and paste with one command </br>
```
wget https://get.gear.rs/gear-nightly-linux-x86_64.tar.xz && \
tar xvf gear-nightly-linux-x86_64.tar.xz && \
rm gear-nightly-linux-x86_64.tar.xz
```
### Copy gear executable to /usr/bin directory 
```
cp gear /usr/bin
```
### With the help of nano we create a service file gear-node.service
```
nano /etc/systemd/system/gear-node.service
```
### Paste the following data into the open nano editor </br>
Instead of "NodeName" - specify the name of your node
```
[Unit]
Description=Gear Node
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/
ExecStart=/usr/bin/gear --name "NodeName" --telemetry-url "ws://telemetry-backend-shard.gear-tech.io:32001/submit 0"
Restart=always
RestartSec=3
LimitNOFILE=10000

[Install]
WantedBy=multi-user.target
```
Exit nano by saving changes to the file </br>
Ctrl+X, Ctrl+Y, Enter
### Starting the service
```
systemctl daemon-reload
```
```
systemctl enable gear-node
```
```
systemctl start gear-node
```
You can check the success of the deployed node on the [Telemetry](https://telemetry.gear-tech.io) website. </br>
Follow the link, type the name of the node.
### Checking the logs:
```
journalctl -n 100 -f -u gear-node
```
## Update Gear node
```
wget https://get.gear.rs/gear-nightly-linux-x86_64.tar.xz
```
```
tar -xvf gear-nightly-linux-x86_64.tar.xz -C /usr/bin
```
```
rm gear-nightly-linux-x86_64.tar.xz
```
```
sudo systemctl restart gear-node
```
## Removing a Gear node
Note that once a node is deleted, you won't be able to fully restore it. See the article [Backup and Restore](https://wiki.gear-tech.io/docs/node/backup-restore) for important data to be backed up.
```
sudo systemctl stop gear-node
```
```
sudo systemctl disable gear-node
```
```
sudo rm -rf /root/.local/share/gear
```
```
sudo rm /etc/systemd/system/gear-node.service
```
```
sudo rm /usr/bin/gear
```

## In the case when the storage space is over, use the following command
### Checking how much space the Gear blockchain database takes up
```
du -h $HOME/.local/share/gear/chains/gear_staging_testnet_v4/db/full
```
## Freeing up space
### Stopping the node
```
systemctl stop gear-node
```
### Cleaning up the cache
```
gear purge-chain
```
### Launching the node
```
systemctl start gear-node
```
# Additional Information
Viewing the Gear Unique Node ID
```
hexdump -e '1/1 "%02x"' /root/.local/share/gear/chains/gear_staging_testnet_v4/network/secret_ed25519
```
Override key to use new server with old Gear ID </br>
After the = sign, specify your key
```
gear --node-key=42bb2fdd46edfa4f41a5f0f9c1a5a1d407a39bafbce6f07456a2c8d9963c8f5c
```

