このAPIはリードのリストを取得します。  
アクセストークンのみ指定した場合は、そのアカウントが持っているすべてのリードを取得します。メールアドレスやリードIDなどいくつかのフィルターで抽出することもできます。  

#### HTTP種類 : GET  
#### URL : [BASE URL]/SpringRest/customer/get  
#### HTTP戻り値 : JSON

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


### 1. アカウントに属するすべてのリードを取得する（アクセストークンのみ指定）  

#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|

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

### 2. メールアドレスに基づいてリードを取得する  

#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|email	|String	|True|  

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

### 3. リードIDに基づいてリードを取得する   

#### パラメータ : 
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|customerId	|Integer[]	|True|

### 4. キャンペーンに基づいてリードを取得する  

#### パラメータ : 

| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaigns	|Integer[]	|True|