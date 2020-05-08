# 入门指南

本指南适用于希望通过 Unity 接入防沉迷功能的发布商。  


## 前提条件  

- 在 iOS 上部署  
  - Xcode 10 或更高版本  
  - iOS 8.0 或更高版本  
  - [CocoaPods](https://guides.cocoapods.org/using/getting-started.html)  

- 在 Android 上部署
  - 定位到 Android API 级别 14 或更高级别  

- [Demo](https://github.com/yumimobi/AntiAddictionSystem-Unity)

## 下载防沉迷 Unity 插件  

借助 防沉迷 Unity 插件，Unity 开发者无需编写 Java 或 Objective-C 代码，即可轻松地在 Android 和 iOS 应用上实现防沉迷的功能。  
该插件提供了一个 C# 界面，用于 Unity 项目中 C# 脚本使用防沉迷功能。

请通过如下链接下载该插件的 Unity 软件包，或在 GitHub 上查看其代码。

[下载插件](https://github.com/yumimobi/AntiAddictionSystem-Unity/releases/download/1.1.0/AntiAddictionSystem.unitypackage)  
[查看源代码](https://github.com/yumimobi/AntiAddictionSystem-Unity) 

### 导入防沉迷 Unity 插件

在 Unity 编辑器中打开您的项目，然后依次选择 Assets > Import Package > Custom Package，并找到您下载的 AntiAddictionSystem.unitypackage 文件。  

<img src='resources/add_custom_package.png'>

确保选择所有文件，然后点击 Import。

<img src='resources/import_custom_package.png'>  

### 加入防沉迷 SDK  

防沉迷 Unity 插件随 Unity Play [服务解析器库](https://github.com/googlesamples/unity-jar-resolver)一起发布。 此库旨在供需要访问 Android 特定库（例如 AAR）或 iOS CocoaPods 的所有 Unity 插件使用。它为 Unity 插件提供了声明依赖项的功能，然后依赖项会被自动解析并复制到 Unity 项目中。

请按照下列步骤操作，确保您的项目包含防沉迷 SDK。 

#### 部署到 iOS   

您无需执行其他步骤即可将防沉迷 SDK 加入 Unity 项目中。  
*注意：iOS 依赖项的标识是通过 CocoaPods 完成的，而 CocoaPods 是构建过程完成后的一个运行步骤。*  

#### 部署到 Android  

在 Unity 编辑器中，依次选择 Assets > Play Services Resolver > Android Resolver > Resolve。  
Unity Play 服务解析器库会将声明的依赖项复制到 Unity 应用的 Assets/Plugins/Android 目录中。  

*注意：防沉迷 Unity 插件依赖项位于 Assets/AntiAddictionSystem/Editor/AntiAddictionSystemDependencies.xml 中*  

<img src='resources/force_resolve.png'>  


# 快速接入
## 横幅  
### 创建 AntiAddictionSDK

```c#
using System;
using UnityEngine;
using AntiAddictionSystem.Api;
using UnityEngine.UI;

public class AntiAddictionDemoScript : MonoBehaviour
{


AntiAddictionSDK antiAddictionSDK;

  void Start() 
  {
    antiAddictionSDK = new AntiAddictionSDK();
    antiAddictionSDK.OnPrivacyPolicyShown += HandlePrivacyPolicyShown;
    antiAddictionSDK.OnUserAgreesToPrivacyPolicy += HandleUserAgreesToPrivacyPolicy;
    antiAddictionSDK.OnLoginSuccess += HandleLoginSuccess;
    antiAddictionSDK.OnLoginHasBeenShown += HandleLoginHasBeenShown;
    antiAddictionSDK.OnLoginHasBeenDismissed += HandleLoginHasBeenDismissed;
    antiAddictionSDK.OnLoginFail += HandleLoginFail;
    antiAddictionSDK.OnUserAuthVcHasBeenShown += HandleUserAuthVcHasBeenShown;
    antiAddictionSDK.OnUserAuthSuccess += HandleUserAuthSuccess;
    antiAddictionSDK.OnWarningHasBeenShown += HandleWarningHasBeenShown;
    antiAddictionSDK.OnUserClickLoginButton += HandleUserClickLoginButton;
    antiAddictionSDK.OnUserClickQuitButton += HandleUserClickQuitButton;
    antiAddictionSDK.OnUserClickConfirmButton += HandleUserClickConfirmButton;
    antiAddictionSDK.OnLoginFail += HandleLoginFail;
    antiAddictionSDK.OnLogoutCallback += HandleLogoutCallback;
    antiAddictionSDK.OnCanPay += HandleCanPay;
    antiAddictionSDK.OnProhibitPay += HandleProhibitPay;
  }
#region AntiAddiction callback handlers
 //隐私协议回调   
    //隐私协议窗口展示接口
    public void HandlePrivacyPolicyShown(object sender, EventArgs args)
    {
        print("AntiAddiction---HandlePrivacyPolicyShown");
    }
    //用户同意隐私协议
    public void HandleUserAgreesToPrivacyPolicy(object sender, EventArgs args)
    {
        print("AntiAddiction---HandleUserAgreesToPrivacyPolicy");
    }

//登录接口
    //登录成功回调
    public void HandleLoginSuccess(object sender, LoginSuccessEventArgs args)
    {
        String zplayId = args.Message;
        print("AntiAddiction---HandleLoginSuccess: " + zplayId);
    }

    //登录界面展示
    public void HandleLoginHasBeenShown(object sender, EventArgs args)
    {
        statusText.text = "HandleLoginHasBeenShown";
        print("AntiAddiction---HandleLoginHasBeenShown");
    }

    //登录界面关闭
    public void HandleLoginHasBeenDismissed(object sender, EventArgs args)
    {
        statusText.text = "HandleLoginHasBeenDismissed";
        print("AntiAddiction---HandleLoginHasBeenDismissed");
    }

    //登录失败
    public void HandleLoginFail(object sender, EventArgs args)
    {
        statusText.text = "HandleLoginFail";
        print("AntiAddiction---HandleLoginFail");
    }

//实名认证回调
    //实名认证界面展示
    public void HandleUserAuthVcHasBeenShown(object sender, EventArgs args)
    {
        statusText.text = "HandleUserAuthVcHasBeenShown";
        print("AntiAddiction---HandleUserAuthVcHasBeenShown");
    }
    //实名认证通过
    public void HandleUserAuthSuccess(object sender, EventArgs args)
    {
        statusText.text = "HandleUserAuthSuccess";
        print("AntiAddiction---HandleUserAuthSuccess");
    }

//防沉迷SDK提示界面回调
    //展示防沉迷提示界面
    public void HandleWarningHasBeenShown(object sender, EventArgs args)
    {
        statusText.text = "HandleWarningHasBeenShown";
        print("AntiAddiction---HandleWarningHasBeenShown");
    }
    //用户点击防沉迷提示界面上的登录按钮
    public void HandleUserClickLoginButton(object sender, EventArgs args)
    {
        statusText.text = "HandleUserClickLoginButton";
        print("AntiAddiction---HandleUserClickLoginButton");
    }
    //用户点击防沉迷提示界面上的退出游戏按钮
    public void HandleUserClickQuitButton(object sender, EventArgs args)
    {
        statusText.text = "HandleUserClickQuitButton";
        print("AntiAddiction---HandleUserClickQuitButton");
    }

    //用户点击防沉迷提示界面上的确定按钮
    public void HandleUserClickConfirmButton(object sender, EventArgs args)
    {
        statusText.text = "HandleUserClickConfirmButton";
        print("AntiAddiction---HandleUserClickConfirmButton");
    }
    
//注销登录接口回调
    public void HandleLogoutCallback(object sender, EventArgs args)
    {
        statusText.text = "HandleLogoutCallback";
        print("AntiAddiction---HandleLogoutCallback");
    }

//支付检测接口回调
    //允许支付
    public void HandleCanPay(object sender, EventArgs args)
    {
        statusText.text = "HandleCanPay";
        print("AntiAddiction---HandleCanPay");
    }
    //不允许支付
    public void HandleProhibitPay(object sender, EventArgs args)
    {
        statusText.text = "HandleProhibitPay";
        print("AntiAddiction---HandleProhibitPay");
    }

#endregion
}
```  

## 展示隐私政策接口

```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.ShowPrivacyPolicyView();
}
```  

## 登录相关接口
<span style="color:rgb(150,0,0);">
<b>重要提示：</b> 应用必须选择下面的一种登录方式进行登录，登录成功之后，防沉迷SDK会自动弹出用户实名认证界面
</span>

### 展示防沉迷SDK登录界面接口

```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.ShowLoginViewController();
}
```

### 账号&密码进行登录
游戏如果使用游戏自己设计的登录界面，可以将登录界面中用户输入的账号&密码传给防沉迷SDK，使用下面的接口进行登录
```c#
if (antiAddictionSDK != null)
{
    //username：账号
    //password：密码
    antiAddictionSDK.LoginWithUserName(username, password);
}
```

### 三方登录平台进行登录
游戏如果接入了三方(微信，QQ等)平台的登录，可以将三方平台返回的用户唯一标识通过以下的接口进行登录
```c#
if (antiAddictionSDK != null)
{   
    //token：三方平台返回的唯一标识
    //otherID：三方平台返回的用户Id，没有的话传token
    //platformName：平台名称，请联系掌游产品获取
    antiAddictionSDK.LoginWithPlatformToken(token, otherID, platformName);
}
```

### Zplay封装的登录SDK进行登录
游戏如果使用Zplay封装的登录SDK，并且获取到了登录成功之后的ZplayId，请使用下面的接口进行登录
```c#
if (antiAddictionSDK != null)
{
    //zplayID：Zplay封装的登录SDK返回的zplayID
    antiAddictionSDK.LoginWithZplayID(zplayID);
}
```

## 游客实名认证接口
用户登录之前，防沉迷SDK会自动生成一个游客的账号，如果应用需要对游客身份进行实名认证，请使用下面的接口
```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.ShowUserAuthenticationViewController();
}
```

## 注销登录接口
游戏需要进行注销登录操作，请使用下面的接口进行注销实名认证用户账号
```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.LoginOut();
}
```

## 注销登录接口
游戏需要进行注销登录操作，请使用下面的接口进行注销实名认证用户账号
```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.LoginOut();
}
```

## 支付相关接口
### 检测本次用户购买是否可以支付
用户购买之前，调用此接口检测，检测的结果请看防沉迷SDK的回调接口
```c#
if (antiAddictionSDK != null)
{
    //payNumber：用户本次购买的金额，单位分
    antiAddictionSDK.CheckNumberLimitBeforePayment(payNumber);
}
```

### 用户支付成功上报
用户支付成功后，请将用户的支付金额上报给防沉迷系统ß
```c#
if (antiAddictionSDK != null)
{   
    //payNumber：用户本次购买成功的金额，单位分
    antiAddictionSDK.ReportNumberAfterPayment(payNumber);
}
```

## 其他接口
### 获取当前登录状态接口
请使用下面的接口获取当前用户的登录状态
```c#
if (antiAddictionSDK != null)
{
     // 获取当前用户登录状态
    // 0: 未登录
    // 1: 游客
    // 2: 正式用户
    string loginStatus = antiAddictionSDK.GetUserLoginStatus();
}
```

### 获取用户的认证身份
获取当前用户的实名认证身份
```c#
if (antiAddictionSDK != null)
{
    // 获取用户的认证身份
    // 0: 未知
    // 1：已成年
    // 2: 未成年
    string userAuthenticationIdentity = antiAddictionSDK.GetUserAuthenticationIdentity() + "";
}
```

### 游戏退到后台接口
当用户按home键，将游戏退出到后台时，请调用下面的接口

<span style="color:rgb(255,0,0);">
<b>重要提示：</b> 游戏退到后台接口必须调用，不调用会导致防沉迷SDK计算游戏时长错误
</span>

```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.GameOnPause();
}
```

### 游戏恢复前台接口
当用户将游戏恢复到前台时，请调用下面的接口

<span style="color:rgb(255,0,0);">
<b>重要提示：</b> 游戏恢复前台接口必须调用，不调用会导致防沉迷SDK计算游戏时长错误
</span>

```c#
if (antiAddictionSDK != null)
{
    antiAddictionSDK.GameOnResume();
}
```

