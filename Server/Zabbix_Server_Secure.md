![image](https://github.com/user-attachments/assets/b1dc093c-595a-4617-92da-35967c3a7790)---
### TLS기반 Zabbix 연동 보안 (PSK 활용)
### Server info : CentOS 9.0 (Zabbix Server)
### Agent info : Debian 11 (Linux IDS/IPS)
### Zabbix Version : 7.0.7
---

<br>

**Zabbix 전용 키 생성(Zabbix Server)**
```
openssl rand -base64 32 > /root/zabbix.psk
```
**Zabbix 키 등록(Zabbix Server)**
```
1. Zabbix Server 접속 및 로그인(https://127.0.0.1)
2. Monitoring --> Hosts --> Create Host --> Host(호스트 등록에 필요한 값 입력) --> Encryption
3. [Connections to host]를 PSK로 변경
```
![image](https://github.com/user-attachments/assets/e51699b3-cb05-48d0-8571-857fbd9a9f56)



**Zabbix 전용 키 배포(Zabbix Server --> Linux IDS/IPS)**
```
scp /root/zabbix.psk user@172.16.150.10:/home/user/zabbix.psk
```

**Zabbix Agent TLS 설정(Linux IDS/IPS)**
```
#/etc/zabbix/zabbix_agentd.conf
TLSConnect=psk
TLSAccept=psk
TLSPSKIdentity=PSK 001
TLSPSKFile=/etc/zabbix/ssl/zabbix.psk
Server=172.16.150.11
ServerActive=172.16.150.11
```
```
cp /home/user/zabbix.psk /etc/zabbix/ssl/zabbix.psk
```

**Zabbix Agent 재시작(Linux IDS/IPS)**
```
systemctl restart zabbix-agent
```

**연동 결과**
![image](https://github.com/user-attachments/assets/590c100d-11cd-428f-a7b6-97cb88307630)


