#   获取TOKEN方法

```
接口URL: GetAccessToken?format=json

请求方式：POST

```
######  Headers:

```
Content-type: text/json
```

#####  参数

key | valuse
---|---
AppId|登陆APPID（即监控ID）
Nonce|随机字符串
Timestamp|时间戳(精确到秒)
Signature|加密签名

###  signature说明
```
appid+Nonce+Timestamp连接成字符串
采用用appkey做密钥生成HMACSHA1得到HASH后转换成Base64，转换后使用URL编码：UrlEncode

### 例：
 string str=appid+Nonce+Timestamp;

#### .NET参考
HMACSHA1 mac = new HMACSHA1(Encoding.UTF8.GetBytes(secret));

byte[] hash = mac.ComputeHash(Encoding.UTF8.GetBytes(signStr));

return Convert.ToBase64String(hash);

### JAVA加密参考

//SecretKeySpec signingKey = new SecretKeySpec(key, "HmacSHA1");
public static String sign(String secretKey, String sigStr, String sigMethod) throws Y3Exception  
 { 
 String sig = null; 
 try{ 
 Mac mac = Mac.getInstance(sigMethod); 
 byte[] hash; 
 SecretKeySpec secretKeySpec = new SecretKeySpec(secretKey.getBytes(CONTENT_CHARSET), mac.getAlgorithm()); 
   mac.init(secretKeySpec); 
 hash = mac.doFinal(sigStr.getBytes(CONTENT_CHARSET)); 
 sig = DatatypeConverter.printBase64Binary(hash); 
 } catch (Exception e) { 
 throw new TencentCloudSDKException(e.getMessage()); 
 } 
 return sig;  
 } 
 
 
 
### php
/** 
 * sign 
 * 生成签名 
 * @param string $srcStr 拼接签名源文字符串 
 * @param string $secretKey secretKey 
 * @param string $method 请求方法 
 * @return 
 */ 
 public static function sign($srcStr, $secretKey, $method = 'HmacSHA1') 
 { 
 switch ($method) { 
 case 'HmacSHA1': 
 $retStr = base64_encode(hash_hmac('sha1', $srcStr, $secretKey, true)); 
 break; 
 case 'HmacSHA256': 
 $retStr = base64_encode(hash_hmac('sha256', $srcStr, $secretKey, true)); 
 break; 
 default: 
 throw new Exception($method . ' is not a supported encrypt method'); 
 return false; 
 break; 
 } 
   return $retStr; 
 } 

```







#####  接收返回结果

```
{
     MsgTime: 2018/5/11 15:47:37
    errmsg: 10004
    succeed: true
    values: {
  "expires_in": 1526629647,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiIxMDAwMCIsIm1lcmNoYW50aWQiOiIzMzM1MTMwOTEiLCJpYXQiOjE1MjYwMjQ4NDcsImp0aSI6ZmFsc2V9.w9QnT87KanKGMNd4vLLB6xa_rkU4duKHYxY24XoWxTY"

}
}
```



KEY | 值 | 说明
---|---|-------
expires_in| 1526629647|TOKEN过期时间
access_token| "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiIxMDAwMCIsIm1lcmNoYW50aWQiOiIzMzM1MTMwOTEiLCJpYXQiOjE1MjYwMjQ4NDcsImp0aSI6ZmFsc2V9.w9QnT87KanKGMNd4vLLB6xa_rkU4duKHYxY24XoWxTY"|访问TOKEN,TOKEN 只有7天有效
