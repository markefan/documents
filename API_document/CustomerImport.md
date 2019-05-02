#### HTTP種類 : POST  
#### URL : <BASE URL>/SpringRest/customer/import  
#### HTTP戻り値 : JSON  
#### パラメータ : 
| 名 前 |	型	| 必 須 |
|:----:|:---:|:---:|
|access_token|	String|	True|
|list|	File|	True|
|userId|	String|	True|
|format|	String|	True|
  
format パラメータは以下のように指定します：  
`format = name,email,gender,birthday,company_name,phone,department,role,industry,country_id,region,prefecture_id,muncipality_id`

csv データのサンプルは以下の通りです：  
`ancil,abc@gmail.com,0,1990-03-01,ABC Inc,9876543210,IT,Engineers,情報通信業,日本,北海道地方,北海道,札幌市`

gender（性別）は以下のように指定してください：
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
このメソッドを使用するには、CSVファイルのパス、ユーザーID、フォーマットおよびアクセストークンを渡す必要があります。  
ユーザーIDはloginAPIより取得します。フォーマットはCSVファイルのフォーマットと一致していなければなりません。  
以下はこのメソッドの呼び出し例で、APIよりJSON戻り値を返します。  

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