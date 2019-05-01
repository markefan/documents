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
