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