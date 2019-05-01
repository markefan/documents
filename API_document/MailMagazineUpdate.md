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