
# Noco Security Api v0.1

- [List](#List)
	- [server functions](#server-functions)
	- [client functions](#client-functions)
	


# server functions

###**run a new server**
	void runServer(String server_name, String ip, int port)
#### Returns
|Type|Description|
|:--|:--|
|void|-|
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|server_name|String|server name|
|ip|string|server ip or domain name|
|port|int|port number|
#### Examples
```
runServer("noco security server", "www.nomadsecurity.com", 1234);
```

###**read json string**
	String readJsonString()
#### Returns
|Type|Description|
|:--|:--|
|String|json string endcoded with noco security protocol|
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|-|-|-|
#### Examples
```
String protocols = server.readJsonString();
```

###**select a cipher-suite**
	int selectCipherSuite(int type)
#### Returns
|Type|Description|
|:--|:--|
|int|returns whether you selected acceptable cipher-suites or not|
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|type|int|cipher suite 종류|
#### Examples
```
int result = selectCipherSuite(1);
```

###**send json string**
	int sendJsonString(String jsonString)
#### Returns
|Type|Description|
|:--|:--|
|int|returns whether json string is sent successfully or not|	
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|jsonString|String|json string that is converted to noco security protocol |
#### Examples
```
int result = sendCertificate(String jsonString)
```


# client functions

###**connects to the server**
	int connectToServer(String ip, int port)
#### Returns
|Type|Description|
|:--|:--|
|int|returns whether or not the connection to the server is successful|	
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|ip|string|server ip or domain name|
|port|int|port number|
#### Examples
```
int result = connectToServer("www.nocosecurity.com", 1234);
```

###**read json string**
	String readJsonString()
#### Returns
|Type|Description|
|:--|:--|
|String|json string endcoded with noco security protocol|
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|-|-|-|
#### Examples
```
String protocols = server.readJsonString();
```

###**send json string**
	int sendJsonString(String jsonString)
#### Returns
|Type|Description|
|:--|:--|
|int|returns whether json string is sent successfully or not|	
#### Parameters
|Name|Type|Description|
|:--|:--|:--|
|jsonString|String|json string that is converted to noco security protocol |
#### Examples
```
int result = sendCertificate(String jsonString)
```
