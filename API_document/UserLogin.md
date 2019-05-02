## ログイン操作

#### HTTP種類 : POST
#### URL : (BASE URL)**/SpringRest/account/user/login**

#### パラメータ  

| 名 前 |	型 | 必 須 |
|:----:|:---:|:---:|
|Username|String|True|
|Password|String|True|

#### HTTP戻り値 : JSON  

#### JSON戻り値 例：  
```json
{
    "code": 200,
    "message": "LOGIN SUCCESS",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK",
    "account": {
        "accountId": null,
        "name": "",
        "companyName": "",
        "companyUrl": null,
        "creationDate": null
    },
    "user": {
        "userId": null,
        "name": "",
        "firstName": "",
        "lastName": "",
        "role": null
    },
    "auth": {
        "accessToken": "",
        "tokenType": "bearer",
        "refreshToken": "",
        "expiresIn": 3000,
        "scope": "[read, trust, write]"
    }
}
```

#### サンプルコード 
This method used for login, you have to pass username and password as the parameter to this method and it will return the response from API. It contains users details, such as userId, accessToken etc.

```java
private String loginApi(String username, String password) {
	try {
		HttpClient client = new DefaultHttpClient();
		HttpPost httpPost = new HttpPost("<API_BASE_URL>/SpringRest/account/user/login");
		List<NameValuePair> Parameter = new ArrayList<NameValuePair>();
		Parameter.add(new BasicNameValuePair("username", username));
		Parameter.add(new BasicNameValuePair("password", password));
		HttpEntity entity = new UrlEncodedFormEntity(Parameter);
		httpPost.setEntity(entity);
		HTTPResponse response = client.execute(httpPost);
		BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
		String line = "";
		while ((line = rd.readLine()) != null) {
				
		return  line;
		}
	} catch (UnsupportedEncodingException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	} catch (ClientProtocolException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	} catch (IOException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
	return null;
}
```

#### 呼び出し方法

```java
String responseJson = loginApi("admin","admin");
```
  
