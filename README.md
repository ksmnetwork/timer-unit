# Run every day at 5AM for one hour and stop  
```
Thu 2022-03-10 05:00:00 CET | 14h left Wed 2022-03-09 05:01:02 CET | 8h ago | kusama.timer | polkadot.service
```

### Create Timer Unit
```
sudo vi /lib/systemd/system/kusama.timer
```
```
[Unit]
Description=Start Polkadot Daemon at 5AM
RefuseManualStart=no
RefuseManualStop=no

[Timer]
OnCalendar=*-*-* 05:00:00
Persistent=true
Unit=polkadot.service

[Install]
WantedBy=timers.target
```

### Add 'RuntimeMaxSec=3600' to the Polkadot Service UNIT to allow only one hour of the service to be active
```
sudo vi /lib/systemd/system/polkadot.service
```
```
[Unit]
Description=Polkadot Service

[Service]
Type=simple
RuntimeMaxSec=3600

ExecStart=/usr/sbin/polkadot \
--validator \
--name=TIMER-SERVICE \
--pruning=archive \
--chain=kusama \
--database=rocksdb \
--log=info

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl daemon-reload
```
```
systemctl --now enable kusama.timer
```
```
systemctl status kusama.timer
```
```
systemctl list-timers --no-pager --all
```
