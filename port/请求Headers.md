# 请求Headers说明

#####  Headers:

```
Content-type: text/json
access_token：token
Nonce：1233456
Timestamp：11232222444
Signature：dfsfsdfsdfsdfsdf

```

#####  参数

key | valuse
---|---
access_token| 从登陆接口获取的TOKEN
Nonce|随机字符串
Timestamp|时间戳（精确到秒）
signature|加密签名

### signature说明
```
AccessToken+Nonce+Timestamp 连接成字符串
采用用appkey做密钥生成HMACSHA1得到HASH后转换成Base64

### 例：
 string str=AccessToken+Nonce+Timestamp;

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


###  注意：

一定要进行URL编码：UrlEncode





### 说明
所有接口的Headers都必须要传用该参数，否则没法得到鉴权使用接口