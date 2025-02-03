### ZABBIX Agent 설치
- Linux Version : Debian 11
- Zabbix Version : 7.0.7

### Zabbix 리포지토리 설치 명령
```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
dpkg -i zabbix-release_latest_7.2+debian12_all.deb
apt update
```

### Zabbix Agent 설치
```bash
apt install zabbix-agent
```

### Zabbix Agent 시작
```bash
systemctl restart zabbix-agent
systemclt enable zabbix-agent
```
