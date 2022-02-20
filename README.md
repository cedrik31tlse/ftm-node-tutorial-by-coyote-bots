
![Coyote bot logo](https://pbs.twimg.com/profile_images/1437957467225268226/a_qfpwtb_400x400.jpg "Logo Coyote bot logo")
# FTM Node Installer Tutorial

#### created by cedrik31tlse#8672 (Discord) / cedrik31tlse (Telegram)

Tutorial to create a node on Fantom network (FTM) to use with Coyote Chainsniper Linux Edition.

Link to official server of Coyote bots : https://discord.gg/fP3Z4tY92j

This tutorial helps to create a node on FTM network from a fresh install of Ubuntu 20.04.

1. Update all packages

```
apt update && apt upgrade -y
```

2. Create ftm user

```
useradd -m ftm
cd /home/ftm
```

3. Edit start.sh command line

```
nano start.sh
```

4. Paste content in the empty file

```
go-opera/build/opera  --genesis go-opera/build/mainnet.g --datadir ./.opera -http --http.api=net,ftm,eth,debug,web3,txpool --http.port=8545 -ws --ws.api=net,ftm,eth,debug,web3,txpool  --ws.addr=0.0.0.0 --ws.origins='*' --ws.port=8546
```

5. Make start.sh file executable

```
chmod +x start.sh
```

6. Install required packages

```
apt-get install -y build-essential git
```

7. Download and uncompress Golang

```
wget https://dl.google.com/go/go1.15.10.linux-amd64.tar.gz
tar -xvf go1.15.10.linux-amd64.tar.gz -C /usr/local/
```

8.  Add PATH variables

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
```

9. Install Fantom Opera (do it line by line)

```
git clone https://github.com/Fantom-foundation/go-opera.git
cd go-opera/
git checkout release/1.0.2-rc.5
make
cd build/
wget https://opera.fantom.network/mainnet.g
```

10. Download last Snapshot (you can find more recent link in https://docs.fantom.foundation/staking/snapshot-download
```
wget https://download.fantom.network/opera_pruned_3feb22.tgz
tar zxvf opera_pruned_3feb22.tgz
```

11. Chown all files to ftm user

```
chown -R ftm.ftm /home/ftm/*
```

12. Create a service to start the node
```
nano /lib/systemd/system/ftm.service
```

13. Copy paste to service file
```
[Unit]
Description=FTM Full Node
[Service]
User=ftm
Type=simple
WorkingDirectory=/home/ftm
ExecStart=/bin/bash /home/ftm/start.sh
Restart=on-failure
RestartSec=5
[Install]
WantedBy=default.target
```

14. Start service

```
systemctl enable ftm
systemctl start ftm
```

15. Check logs of the node

```
journalctl -u ftm -f
```
