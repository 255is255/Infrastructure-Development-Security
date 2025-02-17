# 가상 기업 인프라 보안 구축 프로젝트

가상의 기업 환경을 기반으로 한 **네트워크 인프라 보안 구축**을 목표로 진행된 프로젝트입니다.</br></br> 
다양한 네트워크 기술과 오픈소스 기반 보안 솔루션 및 모니터링 시스템을 활용해보았고, 완성도를 위해 계속 개선 중에 있습니다!!</br>
(앞으로도 이것 저것 테스트 해볼 예정..)

</br>

## 주요 구성 내용

### 1. GRE over IPSEC VPN 이중화
- 주요 사이트 간 안정적인 데이터 통신을 위해 GRE over IPSEC VPN을 이중화(Flotting Static Routing)로 구성
- 데이터의 기밀성과 무결성을 보장

### 2. ZABBIX 모니터링
- 방화벽과 IDS/IPS의 상태를 실시간으로 모니터링
- 장애 발생 시 즉각적인 알림 전송을 통해 빠른 대응 가능

### 3. IDS/IPS 구축
  - 네트워크 상의 악의적인 활동을 탐지하고 차단 하도록 Snort 기반의 IDS/IPS 구축

### 4. 방화벽 구축
  - 네트워크 내외부로의 트래픽을 효과적으로 관리 및 제어하도록 White/Black 리스트 기반의 방화벽 정책 혼합 사용

### 5. DNS 구축
  - 내부자 응용계층 서비스를 위한 Domain Name System 구성
  - DNS RPZ를 활용한 비인가 도메인 접근 방지 구현

</br>

## 네트워크 구성도

### 주요 네트워크 구성
![Network Security Infra](https://github.com/user-attachments/assets/189db29d-2936-4a61-aeff-b7aa02701862)

- **라우팅** 본사와 지사의 라우팅 시 전 구간 Static Routing 사용
- **본사와 지사 연결** : GRE VPN 터널을 통한 안전한 본사-지사 연결
- **VLAN 분리** : 업무별 트래픽 분리 및 보안 강화

</br>

## 사용 기술 및 도구
- **가상화 도구** : GNS3, Vmware Workstation
- **네트워크 프로토콜** : Static Routing, GRE, IPSEC, VLAN, SNMPv3, NAT, ACL
- **보안 솔루션** : ZABBIX, Snortv3, OpenSSL, Cisco ASA
- **네트워크 장비** : Cisco Router, Cisco L3 Switch
- **운영체제** : Linux CentOS 9.0, Linux Debian 11 
