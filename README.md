## 8 steps to install klayr-core@latest with npm on testnet network

System requirements:
- Ubuntu 22.04
- 4GB RAM
- Some basic configuration to secure the server (sudo user, public & private keys, change ssh port to access the server etc.)

## 1. Install prerequisites
```shell
sudo apt-get update
sudo apt install -y libtool automake autoconf curl build-essential python2-minimal
sudo apt-get -y install wget tar zip unzip ufw npm git curl bash jq nodejs
```

## 2. Firewall
```shell
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
**klayr testnet ports**
```shell
sudo ufw allow "7667/tcp"
sudo ufw allow "7887/tcp"
sudo ufw allow 8778
```
**SSH port**
```shell
sudo ufw allow "YOUR_SSH_PORT/tcp"
```
**Activate firewall**
```shell
sudo ufw --force enable
sudo ufw status
```
*Test a new connection after a system restart*
```shell
sudo shutdown -r now
```

## 3. Install nvm
```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```
*Disconnect & make a new connection*
```shell
nvm install 18
nvm alias default 18
nvm use default
```

## 4. Install klayr-core v3.1.0-rc.1 with npm
```shell
npm i -g klayr-core@latest
```
*Check klayr-core location*
```shell
npm list --global --depth=0
```

## 5. Install pm2 & pm2-logrotate
```shell
npm i -g pm2
pm2 install pm2-logrotate
pm2 set pm2-logrotate:max_size 100M
```

## 6. Start klayr core on testnet network
```shell
klayr-core start --network testnet
```

## 7. Create a new file
```shell
cd && nano core-start.json
```

**Paste the following lines and save with CTRL+X+Y+Enter**
```shell
{
  "name": "klayr-core",
  "script": "klayr-core start --api-ipc -c ~/.klayr/klayr-core/config/config.json",
  "env": {
  "KLAYR_NETWORK": "testnet"
  }
 }
```

## 8. Start & Stop klayr-core
```shell
pm2 start core-start.json
pm2 logs
pm2 stop all
```
