# DNS RPZ

DNS Response Policy Zone은 DNS 응답을 제어하여 악의적이거나 원하지 않는 도메인 접근에 대해 효율적으로 관리할 수 있습니다.

## 소개
인터넷에서 우리가 웹사이트에 접속할 때 www.naver.com 같은 도메인 이름을 입력합니다. 컴퓨터는
이 도메인 이름은 실제 서버의 숫자로 된 IP 주소로 바꿔야 합니다. 이 역할을 하는 것이 바로 DNS(Domain Name System)입니다.

DNS 응답 과정에서 특별한 규칙을 추가하는 것이 DNS RPZ입니다. 관리자는 악성 사이트나 위험하다고 판단되는 도메인에 대해 미리 설정한 규칙을 적용할 수 있습니다. 
만약 어떤 도메인이 해커들이 운영하는 사이트라면, DNS RPZ는 그 도메인에 접속하려는 요청을 미리 정의된 주소나 규칙으로 리다이렉션을 진행합니다.
따라서 반환하는 응답절차가 달라지게 되고 이 절차에 따라서 악성 도메인이나 위험 도메인 접근에 원천 차단할 수 있게 됩니다.


## 사용 예시
현 시스템에선 아래 시나리오를 기반으로 방어하고자 시스템을 구성해보았습니다.
![구성 소개](https://github.com/user-attachments/assets/2d4798ff-5710-49d7-ade3-bfa3c9739cef)

업무시간에 웹서핑을 시도하는 A사원 PC에 대한 외부 접근 차단
![image](https://github.com/user-attachments/assets/f837b1e6-a6fe-4f21-a286-549962d43534)

업무상 불필요한 인트라넷 도메인 접근을 시도하는 B사원 PC에 대한 접근 차단
![image](https://github.com/user-attachments/assets/cc454ce2-851a-4b16-8f61-e51a814b3186)


**/etc/named.conf**
```
allow-recursion { 172.16.150.0/24; 172.16.10.0/24; 172.16.20.0/24; 172.16.30.0/24; 172.16.40.0/24; };
forwarders {
  8.8.8.8; // Google Public DNS
};
forward only; // 포워딩만 수행 (루트 힌트 비활성화)

ecursion yes;

response-policy {
  zone "rpz";
};
.
.
.
zone "rpz" {
	type master;
	file "/var/named/rpz.zone";
	allow-query { any; }; // RPZ는 모든 쿼리에 대해 작동
	allow-transfer { none; };
};
```
**/etc/named.rfc1912.zones**
```
zone "choihunseo.com" IN {
        type master;
        file "choihunseo.com.zone";
};
```

**/var/named/choihunseo.con.zone**
```
$TTL 3H
@       IN SOA  ns.choihunseo.com. root.choihunseo.com. (
                                        1       ; serial
                                        3H      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum
        NS      ns.choihunseo.com.
        MX 10   mail.choihunseo.com.
        IN A    172.16.150.11
ns      IN A    172.16.150.11
mail    IN A    172.16.150.11
randing IN A    172.16.150.11
web     IN CNAME www
www     IN A    172.16.150.22
```

**/var/named/rpz.zone**
```
$TTL 3H
@       IN SOA  ns.choihunseo.com. root.choihunseo.com. (
                                        3       ; serial
                                        3H      ; refresh
                                        1H      ; retry
                                        1W      ; expire
                                        3H )    ; minimum

    IN NS   ns.choihunseo.com.

; Zabbix 서버 리다이렉션
zabbix.choihunseo.com.rpz.    IN CNAME randing.choihunseo.com.

; Coupang 도메인 리다이렉션
*.coupang.com.rpz.           IN CNAME randing.choihunseo.com.

; 랜딩 페이지
randing.choihunseo.com.   IN A 172.16.150.11
```
