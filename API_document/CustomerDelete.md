# API Document

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
  
---
## リードの削除  

#### HTTP種類 : DELETE  

#### URL : (BASE URL)**/SpringRest/customer/delete**  

#### HTTP戻り値 : JSON  

#### パラメータ  

| 名 前 |	型 | 必 須 |  
|:----:|:----|:----:|  
|access_token|String|True|  
|customerId|Integer[] (複数の場合はカンマで区切る)|True|  

#### JSON戻り値 例：

```json
{
    "code": 200,
    "message": "deleteCustomers deleted successfully : 1",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK"
}
```

#### サンプルコード
This method is used for customer delete, in this method you have to pass customerId and access token. If you need to delete multiple customers you can give its customerIds as comma separated. If sucessfully deleted then method will return boolean true.

```java
private boolean deleteCustomer(String accessToken , String customerId) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpDelete httpDelete = new HttpDelete("<API_BASE_URL>/SpringRest/customer/delete?access_token="
					+ accessToken + "&customerId=" + customerId);

			HTTP戻り値 response = client.execute(httpDelete);

			if (response.getStatusLine().getStatusCode() == 200) {
				return true;
			} else {
				return false;
			}
		} catch (ClientProtocolException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return false;
	}
```
#### 呼び出し方法

```java
boolean delete = deleteCustomer(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "82");
boolean delete = deleteCustomer(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "82,81"); //Multiple customers
```

---
## リードのインポート

#### HTTP種類 : POST  
#### URL : (BASE URL)**/SpringRest/customer/import**
#### HTTP戻り値 : JSON
#### パラメータ : 
| 名 前 |	型	| 必 須 |
|:----:|:---:|:---:|
|access_token|	String|	True|
|list|	File|	True|
|userId|	String|	True|
|format|	String|	True|

For cxm format parameter value is  
`format = name,email,gender,birthday,company_name,phone,department,role,industry,country_id,region,prefecture_id,muncipality_id`

Sample csv data giving below  
`ancil,abc@gmail.com,0,1990-03-01,ABC Inc,9876543210,IT,Engineers,情報通信業,日本,北海道地方,北海道,札幌市`

For gender
```
0 – Male
1 – Female
2 – Other
```
#### JSON戻り値 例：
```json 
{
    "code": 200,
    "message": "save success",
    "status": "200",
    "generatedId": null,
    "generatedIds": [
        78
    ],
    "statusObject": "OK",
    "errorCode": null,
    "errorLineNumber": null,
    "errorFieldName": null,
    "errorFieldValue": null,
    "errorMessage": null
}
```

#### サンプルコード
This method used for customer import in this method you have to pass the csv files File object, userId, format and access token. Where userId will get while calling login API. Format is in which format we are created csv file. Example of calling this method is given below. Method will return the Json response from API.

```java
private String csvImport(String accessToken , File csvFile, String userId, String format) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpPost httpPost = new HttpPost("<API_BASE_URL>/SpringRest/customer/import");
			HttpEntity entity = MultipartEntityBuilder.create().setMode(HttpMultipartMode.BROWSER_COMPATIBLE)
					.addBinaryBody("list", csvFile).addTextBody("access_token", accessToken)
					.addTextBody("userId", userId).addTextBody("format", format).build();
			httpPost.setEntity(entity);
			HTTP戻り値 response = client.execute(httpPost);
			BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
			String line = "";
			while ((line = rd.readLine()) != null) {

				return line;
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
String Response = csvImport(new File(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "/home/bquser/Downloads/cxm.csv"), "1",
"name,email,gender,birthday,company_name,phone,department,role,industry,country_id,region,prefecture_id,muncipality_id");
```

---
## リードの拡張インポート

This api is used to import customers to MIP. Using parameter doMergeCsvWithDb , if existing customers is in uploading csv then it will be updated. This update is done based on ext_customer_id.

#### HTTP種類 : POST
#### URL : (BASE URL)**/SpringRest/customer/advanced/import**
#### HTTP戻り値 : JSON
#### パラメータ 
| 名 前 |	型	| 必 須 |
|:----:|:---:|:---:|
|access_token	|String	|True|
|list	|File	|True|
|userId	|String	|True|
|format	|String	|True|
|doMergeCsvWithDb	|Boolean	|True|

For cxm format parameter value is  
`
format = name,email,gender,birthday,company_name,phone,department,role,industry,country_id,region,prefecture_id,muncipality_id,ext_customer_id
`  

Sample csv data giving below  
`
ancil,abc@gmail.com,0,1990-03-01,ABC Inc,9876543210,IT,Engineers,情報通信業,日本,北海道地方,北海道,札幌市,10
`  

For gender
```
0 – Male
1 – Female
2 – Other
```

#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": "save success",
    "status": "200",
    "generatedId": null,
    "generatedIds": [
        78
    ],
    "statusObject": "OK",
    "errorCode": null,
    "errorLineNumber": null,
    "errorFieldName": null,
    "errorFieldValue": null,
    "errorMessage": null
}
```

#### サンプルコード
This method used for customer advanced import in this method you have to pass the csv files File object, userId, format and access token. Where userId will get while calling login API. Format is in which format we are created csv file. Example of calling this method is given below. Method will return the Json response from API.

```java
private String csvAdvancedImport(String accessToken , File csvFile, String userId, String format) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpPost httpPost = new HttpPost("<API_BASE_URL>/SpringRest/customer/advanced/import");
			HttpEntity entity = MultipartEntityBuilder.create().setMode(HttpMultipartMode.BROWSER_COMPATIBLE)
					.addBinaryBody("list", csvFile).addTextBody("access_token", accessToken)
					.addTextBody("userId", userId).addTextBody("format", format).addTextBody(“doMergeCsvWithDb”,”true”).build();
			httpPost.setEntity(entity);
			HTTP戻り値 response = client.execute(httpPost);
			BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
			String line = "";
			while ((line = rd.readLine()) != null) {

				return line;
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
String Response = csvAdvancedImport(new File(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "/home/bquser/Downloads/cxm.csv"), "1","name,email,gender,birthday,company_name,phone,department,role,industry,country_id,region,prefecture_id,muncipality_id,ext_customer_id");
```

---
## 既存キャンペーンへのリード追加

#### HTTP種類 : PUT
#### URL : (BASE URL)**/SpringRest/campaign/mailmagazine/update**
#### HTTP戻り値 : JSON
#### パラメータ :
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaignId	|Integer	|True|
|customers	|Integer[] (カンマで区切ったcustomerid)	|True|

While updating customers currently given customers will be the target customers. Updating existsing customers with current given customers.

#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": " CampaignCustomers created ,",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK"
}
```

#### サンプルコード
This method is used for adding customers to existing campaigns. In this method you have to pass campaignId, customers (multiple customers id comma separated) and access token. While calling this method currently given customers will be set as target customer for that campaign, existing customers will be removed. This method will return boolean true if update is success.

```java
private boolean addTargetCustomerToCampaign(String accessToken , String campaignId, String customers) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpPut httpPut = new HttpPut(
					"<API_BASE_URL>/SpringRest/campaign/mailmagazine/update?access_token=" + accessToken
							+ "&campaignId=" + campaignId + "&customers=" + customers);

			HTTP戻り値 response = client.execute(httpPut);

			if (response.getStatusLine().getStatusCode() == 200) {
				return true;
			} else {
				return false;
			}
		} catch (ClientProtocolException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return false;
	}
```

#### 呼び出し方法
```java
boolean update = addTargetCustomerToCampaign( “98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "527", "25,81");
```

---
## メールステータス

To get the list of customers which we send  campaign, mail open , mail link click  of that campaign, we can use the below given campaign

#### HTTP種類 : GET
#### URL : (BASE URL)**/SpringRest/campaign/status/get**
#### HTTP戻り値 : JSON
#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaignId	|Integer	|True|
|status	|Integer[] 	|True|

customerId (Type integer) parameter is also supports this api, it will give all campaigns of that customer having given status(if status given as 1 then will list customer mail opened campaigns).

Status ids of each events
```
Mail Send – 0
Mail Open – 1
Mail Link Click – 5
Mail Bounce – 2
Mail Fail – 8
```

#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": "Success",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK",
    "pagination": {
        "offset": 0,
        "nextOffset": 0,
        "prevOffset": 0,
        "limit": 1,
        "total": 1,
        "nextUrl": null,
        "prevUrl": null,
        "accountId": null,
        "message": "Data available",
        "sort": null
    },
    "data": [
        {
            "campaign": {
                "campaignId": ,
                "type": {
                    "id": 1,
                    "text": "Mail Magazine"
                },
                "title": "",
                "description": null
            },
            "customer": {
                "customerId": ,
                "firstName": null,
                "lastName": null,
                "name": "",
                "email": "",
                "gender": {
                    "id": 0,
                    "text": "Male"
                },
                "birthday": null,
                "company": null,
                "statusNew": null,
                "statusActive": null,
                "statusHot": null,
                "statusRankup": null,
                "statusRankdown": null,
                "statusWatch": null,
                "statusCWatch": null
	"primaryStatus": {}
            },
            "status": {
                "id": 1,
                "text": "開封"
            },
            "responseStatus": null,
            "creationDate": null
        }
    ]
}
```


### 1. Customers list status by giving campaign id and status

This method is used for getting customers who have received mail, mail open, mail link click, bounce etc based on status parameter. You have to pass campaignId, status (status values are mentioned above) and access token. It will return the JSON response from API.

#### サンプルコード

```java
private String MailStatusGet(String accessToken , String campaignId, String status) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/campaign/status/get?access_token="
					+ accessToken + "&campaignId=" + campaignId + "&status=" + status);

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = MailStatusGet(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "525", "0");
```

### 2. Customer status by giving campaign id and customerId

This method is used to get different status of a customer in campaign. It will return the response Json from api.

#### サンプルコード
```java
private String MailStatusGet(String accessToken , String campaignId, Int customerId) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/campaign/status/get?access_token="
					+ accessToken + "&campaignId=" + campaignId + "&customerId=" + customerId);

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = MailStatusGet(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "525", 25);
```

---
## メール拡張ステータス

To get the list of customers which we send  campaign, mail send but not open , mail open but not link click  of that campaign, we can use the below given campaign

#### HTTP種類 : GET
#### URL : (BASE URL)**/SpringRest/campaign/status/advanced/get**
#### HTTP戻り値 : JSON
#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaignId	|Integer	|True|
|status	|Integer[] 	|True|

Possible status values
```
Mail send but not open – 0
Mail open but not link clicked - 1
```

#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": "Success",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK",
    "pagination": {
        "offset": 0,
        "nextOffset": 0,
        "prevOffset": 0,
        "limit": 1,
        "total": 1,
        "nextUrl": null,
        "prevUrl": null,
        "accountId": null,
        "message": "Data available",
        "sort": null
    },
    "data": [
        {
            "campaign": {
                "campaignId": ,
                "type": {
                    "id": 1,
                    "text": "Mail Magazine"
                },
                "title": "",
                "description": null
            },
            "customer": {
                "customerId": ,
                "firstName": null,
                "lastName": null,
                "name": "",
                "email": "",
                "gender": {
                    "id": 0,
                    "text": "Male"
                },
                "birthday": null,
                "company": null,
                "statusNew": null,
                "statusActive": null,
                "statusHot": null,
                "statusRankup": null,
                "statusRankdown": null,
                "statusWatch": null,
                "statusCWatch": null
	"primaryStatus": {}
            },
            "status": {
                "id": 1,
                "text": "開封"
            },
            "responseStatus": null,
            "creationDate": null
        }
    ]
}
```

#### サンプルコード
This method is used to get list of customers status based on campaign and status. This method will return Response Json from Api.

```java
private String MailStatusLatestGet(String accessToken , String campaignId, String status) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/campaign/status/advanced/get?access_token="
					+ accessToken + "&campaignId=" + campaignId + "&status=" + status);

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = MailStatusLatestGet(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, "525", "0");
```

---
## リードの取得

This api used to get customer list. If you are passing only access token to this api you will get all the customers in this account. There are several filters like, email, customerId etc. 

#### HTTP種類 : GET
#### URL : (BASE URL)**/SpringRest/customer/get**
#### HTTP戻り値 : JSON

To list all customers details in the account パラメータ need to pass.

#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|


To list customer details based on email

#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|email	|String	|True|

To list Customers details based on customerId

#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|customerId	|Integer[]	|True|

To list customers details based on a campaign.

#### パラメータ : 

| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaigns	|Integer[]	|True|


#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": "Success",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK",
    "pagination": {
        "offset": 0,
        "nextOffset": 0,
        "prevOffset": 0,
        "limit": 1,
        "total": 1,
        "nextUrl": null,
        "prevUrl": null,
        "accountId": null,
        "message": "Data available",
        "sort": null
    },
    "data": [
        {
            "customerId": ,
            "firstName": null,
            "lastName": null,
            "name": "",
            "user": ,
            "customerKey": "",
            "anonymous": null,
            "shareLevel": null,
            "fnameFurigana": null,
            "lnameFurigana": null,
            "alias": null,
            "email": "",
            "company": ,
            "gender": ,
            "birthday": "",
            "phone": "",
            "mobile": null,
            "fax": null,
            "approvedStaff": null,
            "department": "",
            "prefecture": "",
            "country": "",
            "city": "",
            "address": null,
            "url": null,
            "referenceUrl": null,
            "role": ,
            "job": null,
            "funnel": ,
            "category": null,
            "primaryStatus": ,
            "mailStatus": null,
            "customerAuthority": null,
            "customerType": null,
            "customerDealType": null,
            "customerDealStatus": null,
            "municipality": ,
            "region": ,
            "prefectureMaster":,
            "countryMaster": ,
            "statusNew": null,
            "statusActive": null,
            "statusHot": null,
            "statusRankup": null,
            "statusRankdown": null,
            "statusWatch": null,
            "statusCWatch": null,
            "statusOpen": null,
            "firstVisit": null,
            "lastVisit": null,
            "visitFrequency": null,
            "customFields": null,
            "landingPage": null,
            "leadSource": null,
            "score": null,
            "categories": [],
            "segments": [],
            "customerMails": null,
            "creationDate": "",
            "modificationDate": ""
        }
    ]
}
```


### 1. This method is used to list all customers in that account. It will return Response Json from Api.

#### サンプルコード
```java
private String getAllCustomers(String accessToken) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/customer/get?access_token="
					+ accessToken );

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = getAllCustomers(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”);
```

### 2. This method is used to list all customers based on email. It will return Response Json from Api.

#### サンプルコード
```java
private String getCustomerByEmail(String accessToken, String email) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/customer/get?access_token="
					+ accessToken+”&email=”+email );

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = getCustomerByEmail(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, “abc@gmail.com”);
```


---
## キャンペーン対象リードの取得

This api is used to get target customers of a campaign. You have to pass access_token and campaignId to the api.

#### HTTP種類 : GET
#### URL : (BASE URL)**/SpringRest/campaign/component/customer/get**
#### HTTP戻り値 : JSON
#### パラメータ :
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaignId	|Intger	|True|

#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": "Success",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK",
    "pagination": {
        "offset": 0,
        "nextOffset": 0,
        "prevOffset": 0,
        "limit": 1,
        "total": 1,
        "nextUrl": null,
        "prevUrl": null,
        "accountId": null,
        "message": "Data available",
        "sort": null
    },
    "data": [
        {
            "customerId": null,
            "firstName": null,
            "lastName": null,
            "name": "ancil",
            "email": null,
            "gender": null,
            "birthday": null,
            "company": null,
            "statusNew": null,
            "statusActive": null,
            "statusHot": null,
            "statusRankup": null,
            "statusRankdown": null,
            "statusWatch": null,
            "statusCWatch": null,
            "primaryStatus": {}
        }
    ]
}
```

#### サンプルコード

This method is used to get the customers of a particular camapign. It will return the response json from Api.

```java
private String getCampaignCustomers(String accessToken, int campaignId) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/campaign/component/customer/get?access_token="
					+ accessToken+”&campaignId=”+campaignId );

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = getCampaignCustomers(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, 526);
```

---
## セグメントリードの取得

This api is used to get the list of customers in a segment. We need to pass access token and segment ids.It will retun customers in that segments.

#### HTTP種類 : GET
#### URL : (BASE URL)**/SpringRest/segment/getCustomerBySegment**
#### HTTP戻り値 : JSON
#### パラメータ :
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|accessToken	|String	|True|
|segmentId	|Intger[] (複数の場合はカンマで区切る)	|True|

#### JSON戻り値 例：
```json
{
    "code": 200,
    "message": "Success",
    "status": "OK",
    "generatedId": null,
    "generatedIds": null,
    "statusObject": "OK",
    "pagination": {
        "offset": 0,
        "nextOffset": 0,
        "prevOffset": 0,
        "limit": 2,
        "total": 2,
        "nextUrl": null,
        "prevUrl": null,
        "accountId": null,
        "message": "Data available",
        "sort": null
    },
    "data": [
        {
            "segment": {
                "segmentId": ,
                "title": "",
                "type": {
                    "id": 0,
                    "text": "Mixed"
                },
                "condition": {
                    "id": 1,
                    "text": "AND"
                },
                "innerSegmentEnable": false,
                "color": null
            },
            "customer": [{},{}]
        }
    ]
}
```

#### サンプルコード

This method is used to get list of customers in a segment. You need to pass access token and segment ids to this method. It will return response Json from Api.

```java
private String getCustomerBySegments(String accessToken, String segmentIds) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpGet httpGet = new HttpGet("<API_BASE_URL>/SpringRest/segment/getCustomerBySegment?access_token="
					+ accessToken+”&segmentId=”+ segmentIds );

			HTTP戻り値 response = client.execute(httpGet);

			if (response.getStatusLine().getStatusCode() == 200) {
				BufferedReader rd = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
				String line = "";
				while ((line = rd.readLine()) != null) {

					return line;
				}
			}
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
String response = getCustomerBySegments(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, “28”); // one  segment

String response = getCustomerBySegments(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, “28,29”); // multiple segments passing
```
