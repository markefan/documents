キャンペーンで送付したメールが未開封または未クリックであるリードリストを取得します。

#### HTTP種類 : GET  
#### URL : [BASE URL]/SpringRest/campaign/status/advanced/get  
#### HTTP戻り値 : JSON  
#### パラメータ :  
| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaignId	|Integer	|True|
|status	|Integer[] 	|True|

ステータス：  
```
メール未開封 – 0
メールを開封したがリンク未クリック - 1
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
このメソッドは指定したキャンペーンIDとステータスに合致したリードのリストを取得します。このメソッドはAPIからJSONレスポンスを返します。  

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