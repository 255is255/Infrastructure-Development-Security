### 구축 구간

![image](https://github.com/user-attachments/assets/77fec8bb-2b08-4192-a5eb-b4aabe850949)

### 본사/지사 간 외부 패킷과 내부 패킷 차이

![image](https://github.com/user-attachments/assets/6cafdb43-1d38-473c-9f60-d529a99c1721)

- 지사 -> 본사 통신 결과
  - 내부 구간에서 호스트간 ICMP 통신으로 패킷에 별도 Secuirty Payload가 없는 점 확인
  - 외부 구간에서 ESP(Encapsulating Security Payload) 패킷으로 인캡슐레이션 되어 주고 받는 점 확인

### 본사/지사 간 암호화 된 패킷 확인

![image](https://github.com/user-attachments/assets/7dd2e411-d605-4fe7-bf26-284d09b985cc)


### Floating Static Routing 기반 터널 전환

![image](https://github.com/user-attachments/assets/4b520ce7-cff9-49ea-8d32-bb0cfef3490f)

- Tunnel1이 연결된 물리적 인터페이스를 일시 중지 하였을 때 수 초 이내 Tunnel2로 전환
- Floating Static Routing : 가중치 기반 정적 라우팅
