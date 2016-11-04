# 两个APP之间跳转传值方法

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
