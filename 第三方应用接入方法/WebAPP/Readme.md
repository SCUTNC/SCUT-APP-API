# 华南理工APP第三方WebAPP接入

## APP在华南理工平台上发布需要提供以下材料：
```
1.应用名字；
2.应用链接地址url；
3.作者名：用于标识开发方。
4.app logo，要求：尺寸长宽比1：1，图片像素不低于170x170px，图片大小控制在300K以内。
```

##  不需要免登陆接入
```
（1）只提供接入需要的资料即可。
```
![step1](https://github.com/SCUTNC/SCUT-APP-API/blob/master/第三方应用接入方法/WebAPP/image/图片1.png)

##  应用免登录接入
```
（1）提供接入需要的资料。
（1）应用开发方做根据华工统一认证的token，“iPlanetDirectotyPro"，去获取用户的信息（统一认证接口另行提供）
（2）管理平台这边会设置好一个私密action，传参时，判断action是否正确，若正确，在访问web应用的时候会在cookie上面带上统一认证的token。
     （action由官方保管，不对外提供）
```
![step2](https://github.com/SCUTNC/SCUT-APP-API/blob/master/第三方应用接入方法/WebAPP/image/图片2.png)
