# Android 第三方应用接入

## APP在华南理工平台上发布需要提供以下材料：
1. APP包名，例：com.scut.app.activity
2. APP启动Action: com.allimu.app.sport.main
3. 应用下载地址url。
4. 作者名：用于标识开发方。
5. app logo

##  应用跳转中获取个人信息接口
- 华南理工应用跳转到另外应用中，先通过`getIntent().getLongExtra(“userId”,-1)`;
获取用户`Id`，`getIntent().getStringExtra(“sid”)`;
- cookie 设置`sid`为之前获取的`sid`，如：在`volley`中设置头部，注意：一定要设置`cookie`


```java
@Override
public Map<String, String> getHeaders() throws AuthFailureError {
    String sid = Constant.Cookie;
    if (sid != null && !sid.isEmpty()) {
        HashMap<String, String> headers = new HashMap<String, String>();
        headers.put("Cookie", "sid=" + sid);
        return headers;
    }
    return super.getHeaders();
}
```

### 接口调用
- method：POST
- url:http://imuserver.allimu.net:8080/cloud-imu-server/user/User_detail.html

#### 需要传递的参数：
```
timestamp：当前时间戳(String.valueOf(System.currentTimemillis()))
spId:（另外告知）
appkey:(另外告知)
userId:传过来的userId
ver:1
clientSign:这个是结合上面的参数根据md5加密生成的一个参数值
```
#### getSign方法（`需要MD5文件`）
```java
public static String getSign(HashMap<String, String> map) {
    StringBuilder sign = new StringBuilder();
    Collection<String> keySet = map.keySet();
    List<String> keyList = new ArrayList<String>(keySet);
    //对key键值按字典升序排序
    Collections.sort(keyList);
    for (String key : keyList) {
        sign.append(map.get(key));
    }
    sign.append(Config.APP_SECRET);
    ImuLog.d("sign", sign.toString());
    String md5;
    try {
        md5 = MD5.getMD5(sign.toString().getBytes("utf-8")).toLowerCase();
    } catch (UnsupportedEncodingException e) {
        md5 = MD5.getMD5(sign.toString().getBytes()).toLowerCase();
    }
    return md5;
}
```
