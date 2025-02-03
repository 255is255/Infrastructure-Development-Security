## ZABBIX 서버 설치 과정
- Linux Version : CentOS 9.0
- Zabbix Version : 7.0.7

```bash
useradd zabbix
groupadd zabbix
usermod zabbix -G zabbix
```

### /etc/yum.repos.d/epel.repo
```bash
[epel]
...
excludepkgs=zabbix*
```

### 패키지 설치
```bash
rpm -Uvh https://repo.zabbix.com/zabbix/7.0/centos/9/x86_64/zabbix-release-latest-7.0.el9.noarch.rpm
dnf clean all
```

### Zabbix 서버 설치
```bash
dnf install zabbix-server-mysql zabbix-web-mysql zabbix-apache-conf zabbix-sql-scripts zabbix-selinux-policy zabbix-agent
```

### DB 셋팅
```sql
mysql -uroot -p
  password
  mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
  mysql> create user zabbix@localhost identified by 'password';
  mysql> grant all privileges on zabbix.* to zabbix@localhost;
  mysql> set global log_bin_trust_function_creators = 1;
  mysql> quit;
```

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

```sql
mysql -uroot -p
password #패스워드 입력
  mysql> set global log_bin_trust_function_creators = 0;
  mysql> quit;
```

### /etc/zabbix/zabbix_server.conf 수정

```bash
DBPassword=password
```

```bash

### 시스템 재시작
systemctl restart zabbix-server zabbix-agent httpd php-fpm
systemctl enable zabbix-server zabbix-agent httpd php-fpm
```
