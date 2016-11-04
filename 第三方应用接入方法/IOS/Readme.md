# iOS第三方应用接入

## IOS应用发布需提供资料

1. APP别名,在`URL types`>`Item 0`>`URL Schemes`获取
2. 应用下载地址url。
3. 作者名：用于标识开发方。
4. app logo

## 被跳转的APP上的操作步骤

要实现跳转免登陆，在被跳转的APP的AppDelegate中加入下面代码，这个方法会在APP被外部链接打开的时候调用，可以在这里获取打开本应用的APP打开的URL，里面的参数可以获取出来
```
(请重盟同事补充)
```
获取到用户的信息字典，里面有用户ID，userName，密码等数据，通过这些数据就可以实现免登陆。

## 两个APP之间跳转传值方法

两个APP之间的跳转是通过```[[UIApplication sharedApplication] openURL:url]```这种方式来实现的。

1. 首先设置第一个APP的url地址
2. 接着设置第二个APP的url地址
3. 需要跳转的时候
```
NSString *urlString = [NSString stringWithFormat:@"AppJumpSecond://%@",textField.text]; [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
```
这里将textField的文字也传过去,同样的，在第二个页面也是如此
```
NSString *urlString = [NSString stringWithFormat:@"AppJumpFirst://%@",textField.text]; [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
```
这样就能相互跳转了

4. 处理传过去的数据
在上面传了textField的数据，接收时在AppDelegate的`- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation`方法里。

在`AppDelegate`里设置属性`@property (nonatomic, strong) RootViewController *rvc;`在`didFinishLaunchingWithOptions`方法里添加:
```
self.rvc = [[RootViewController alloc] init]; UINavigationController *nc = [[UINavigationController alloc] initWithRootViewController:self.rvc]; self.window.rootViewController = nc;
```
添加代码块
```
 - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {     self.rvc.textField.text = [[url host] stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];     return YES; }
```
使得textField显示另一个页面传过来的数据。

