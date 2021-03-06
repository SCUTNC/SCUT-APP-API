# iOS第三方应用接入

## IOS应用发布需提供资料

1. APP别名,在`URL types`>`Item 0`>`URL Schemes`获取
2. 应用下载地址url。
3. 作者名：用于标识开发方。
4. app logo，要求：尺寸长宽比1：1，图片像素不低于170px170px，图片大小控制在300K以内

## 被跳转的APP上的操作步骤

要实现跳转免登陆，在被跳转的APP的AppDelegate中加入下面代码，这个方法会在APP被外部链接打开的时候调用，可以在这里获取打开本应用的APP打开的URL，里面的参数可以获取出来
```objective-c
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation{
    //先把app跳转传过来的url字符串化
    NSString *urlStr = [url absoluteString];
    //对字符进行截取，获取所需要的账号密码，“HGWiFiConnect://”这个设置是不变的
    if ([urlStr hasPrefix:@"HGWiFiConnect://"]) {
        urlStr = [urlStr stringByReplacingOccurrencesOfString:@"HGWiFiConnect://" withString:@""];
        if (![urlStr isEqualToString:@""]) {
            NSArray *paramArray = [urlStr componentsSeparatedByString:@"&"];
            NSString *userName = nil;
            NSString *passWord = nil;
            NSMutableDictionary *paramsDic = [[NSMutableDictionary alloc] initWithCapacity:0];
            for (int i = 0; i < paramArray.count; i++) {    
                NSString *str = paramArray[i];
                NSArray *keyArray = [str componentsSeparatedByString:@"="];
	       NSString *key = keyArray[0];
	       NSString *value = keyArray[1];
	       [paramsDic setObject:value forKey:key];
                NSLog(@"key:%@ ==== value:%@", key, value);
                if ([key isEqualToString:@"username"]) {
                    //保存账号
                    userName = value;
                }
                if ([key isEqualToString:@"password"]) {
                    //保存密码
                    passWord = value;}
            }
            //实现免登录的方法
            [self openDoLoginView:userName andPassW:passWord];
        }  
    } 
    return NO;
```

获取到用户的信息字典，里面有用户ID，userName，密码等数据，第三方APP可通过这些数据实现自身的免登陆。

## 两个APP之间跳转传值方法

两个APP之间的跳转是通过```[[UIApplication sharedApplication] openURL:url]```这种方式来实现的。

1) 首先设置第一个APP的url地址
![step1](https://github.com/SCUTNC/SCUT-APP-API/blob/master/%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%94%E7%94%A8%E6%8E%A5%E5%85%A5%E6%96%B9%E6%B3%95/IOS/images/url1.jpg)

2) 接着设置第二个APP的url地址
![step2](https://github.com/SCUTNC/SCUT-APP-API/blob/master/%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%94%E7%94%A8%E6%8E%A5%E5%85%A5%E6%96%B9%E6%B3%95/IOS/images/url2.jpg)

3) 需要跳转的时候
```
NSString *urlString = [NSString stringWithFormat:@"AppJumpSecond://%@",textField.text]; [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
```
这里将textField的文字也传过去,同样的，在第二个页面也是如此
```
NSString *urlString = [NSString stringWithFormat:@"AppJumpFirst://%@",textField.text]; [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
```
这样就能相互跳转了

4) 处理传过去的数据

在上面传了textField的数据，接收时在AppDelegate的`- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation`方法里。

在`AppDelegate`里设置属性`@property (nonatomic, strong) RootViewController *rvc;`在`didFinishLaunchingWithOptions`方法里添加:
```objective-c
self.rvc = [[RootViewController alloc] init]; UINavigationController *nc = [[UINavigationController alloc] initWithRootViewController:self.rvc]; self.window.rootViewController = nc;
```
继续在`AppDelegate`添加代码块
```objective-c
 - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {     self.rvc.textField.text = [[url host] stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];     return YES; }
```
使得textField显示另一个页面传过来的数据。

