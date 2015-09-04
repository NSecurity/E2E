
#**Noco Security Protocols v0.1**

- [List](#List)
	- [E2E Security Protocols](#e2e-security-protocols)
	- [EMS Protocols](#ems-protocols)

# E2E Security Protocols
####**JSON protocol definitions**
 
|구분|설명||필수|참조|server|client|
|:--|:--|:--|:--|:--|:--|:--|
|type(1)|통신종류||O|[1]|O|O|
|session_id(32)|세션ID||O||O|O|
|contents(*)|상세내용||O||O|O|
||random(32)|timestamp(4) + client random(28)|X||O|O|
||cipher_suites_length(1)|가능한 chipher suites 리스트 갯수|X||X|O|
||cipher_suites_list(*)|[symetric-key(1) + public-key(1) + hmac(1)]*갯수|X|[2]|X|O|
||cipher_suites_selected(3)|서버가 선택한 cipher suites|X||O|X|
||server_name(22)|서버 이름|?||?|?|
||certificate_length(3)|서버인증서 길이|X||O|X|
||certificate(*)|서버인증서|X||O|X|
||key_exchange(128)|클라이언트 키 교환|X||X|O|
||finished(128)|data 통신 준비 완료|X||O|O|
||application_data_length(3)|실제 데이터 길이|X||O|O|
||application_data(*)|실제 데이터|X||O|O|  

#### **Sequence Diagram**
```sequence
Client->Server: clientHello
Server->Client: serverHello
Server->Client: certificate
Client->Server: keyExchange
Client->Server: finished
Server->Client: finished
Client->Server: applicationData
Server->Client: applicationData
```

#### **Server To Client**
- send 'serverHello'
> clientHello에 대한 응답으로 server random 값,  클라이언트에서 받은 cipher_suites_list 중 서버가 선택
> 한 값(cipher_suites_selected),  서버 이름 정보를 contents에 담아 전송한다.
 
```
{
	"type": "02",
	"session_id": "00000000...",
	"contents": {
		"random": "0000...",
		"cipher_suites_selected": "01",
		"server_name": "www.nomadconnection.com"
	}
}
```  

 - send 'certificate'
> 서버 인증서를 클라이언트로 보낸다.
```
{
	"type": "03",
	"session_id": "00000000...",
	"contents": {
		"certificate_length": "010101",
		"certificate": "000000..."
	}
}
```  

- send 'finished'
> 클라이언트에서 finished를 받으면 응답으로 보낸다.
> 특정 값을 암호화하여 보낸다.
```
{
	"type": "05",
	"session_id": "00000000...",
	"contents": {
		"finished": "000000..."
	}
}
```
- send 'data'
> master-key로 암호화한 application data를 보낸다.
```
{
	"type": "06",
	"session_id": "00000000...",
	"contents": {
		"finished": "000000..."
	}
}
```

#### **Client To Server**
- send 'clientHello'
> 클라이언트는 서버에 접속한 뒤 client random 값,  지원 가능한 cipher suites 목록(cipher_suites_list),  
> cipher suites  목록 갯수(cipher_suites_list), 서버 이름 정보를 contents에 담아 전송한다.
 
```
{
	"type": "01",
	"session_id": "00000000...",
	"contents": {
		"random": "0000...",
		"cipher_suites_length": "01",
		"cipher_suites_list": "010101",
		"server_name": "www.nomadconnection.com"
	}
}
```  

- send 'keyExchange'
> 클라이언트는 서버 인증서를 받아 공개키를 추출하고 자신의 비밀키를 이 공개키로 암호화 한 후
> pre-master-key를 생성하여 서버로 전송한다.
```
{
	"type": "04",
	"session_id": "00000000...",
	"contents": {
		"key_exchange": "0000..."
	}
}
```

####**[1]통신종류 **
|type|내용|
|:--|:--|
|01|clientHello|
|02|serverHello|
|03|certificate|
|04|keyExchange|
|05|finished|
|06|applicationData|

####**[2]cipher suites**
#####**1st byte(비밀키 암호화 알고리즘)**
|type|내용|
|:--|:--|
|01|AES|

#####**2nd byte(공개키 암호화 알고리즘)**
|type|내용|
|:--|:--|
|01|RSA|

#####**3rd byte(HMAC 해쉬 알고리즘)**
|type|내용|
|:--|:--|
|01|MD5|
|02|SHA1|

#EMS Protocols
####**CMS protocol definition**
|구분|설명||참조|
|:--|:--|:--|:--|
|type|통신종류||[1]|
|contents|상세내용|||
||certificate|인증서||
||enctryped|암호문||
||envaloped|전자봉투||

#### **Sender To Receiver**
- request 'certificate'
> CMS 전송자가 먼저 수신자에게 비밀키 암호화에 필요한 인증서를 요청한다.
 
```
{
	"type": "01"
}
```  

- response 'certificate'
> 인증서 요청에 대해여 응답한다. 
```
{
	"type": "02",
	"contents": {
		"certificate": "000000..."
	}
}
```

- send 'CMS'
> CMS(암호문 + 전자봉투)를 전송한다. 
```
{
	"type": "03",
	"contents": {
		"encrypted": "000000...",
		"enveloped": "000000..."
	}
}
```
####**[1]통신종류**
|type|내용|
|:--|:--|
|01|인증서 요청|
|02|인증서 응답|
|03|CMS 전송|
