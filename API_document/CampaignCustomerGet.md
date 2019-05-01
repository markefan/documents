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
```:json
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

```:java
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
```:java
String response = getCampaignCustomers(“98d9a7ea-8669-45e6-b141-f663c8cb35b8”, 526);
```
