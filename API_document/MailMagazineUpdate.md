既存キャンペーンに指定したリードを追加します。  

#### HTTP種類 : PUT  
#### URL : [BASE URL]/SpringRest/campaign/mailmagazine/update  
#### HTTP戻り値 : JSON  
#### パラメータ :  

| 名 前 |	型	| 必 須 | 
|:----:|:---:|:---:|
|access_token	|String	|True|
|campaignId	|Integer	|True|
|customers	|Integer[] (カンマで区切ったcustomerid)	|True|

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

このメソッドは既存のキャンペーンにリードを追加します。このメソッドを使用するには、キャンペーンID、リードID、アクセストークンを渡す必要があります。複数のリードIDを渡す場合は、カンマで区切ってください。  
このメソッドを実行すると、指定したキャンペーンに既に存在しているリードは除去され、指定したリードが追加されます。
更新が成功すると True の戻り値を返します。

```java
private boolean addTargetCustomerToCampaign(String accessToken , String campaignId, String customers) {
		try {
			HttpClient client = new DefaultHttpClient();
			HttpPut httpPut = new HttpPut(
					"<API_BASE_URL>/SpringRest/campaign/mailmagazine/update?access_token=" + accessToken
							+ "&campaignId=" + campaignId + "&customers=" + customers);

			HttpResponse response = client.execute(httpPut);

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