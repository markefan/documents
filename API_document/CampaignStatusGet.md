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
