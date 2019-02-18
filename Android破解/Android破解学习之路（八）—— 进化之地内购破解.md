原文链接：http://www.cnblogs.com/kexing/archive/2018/06/27/9231454.html

## 前言
## 
最近在TapTap闲逛，看到了进化之地这款游戏，TapTap上有两个进化之地，一个是在TapTap直接购买的，另外一个则是试玩版，玩到中间就会有个购买完整版。  

试玩版连接：https://www.taptap.com/app/33024  

闲来无事，便是下了试玩版来玩，感觉还不错，到中间的时候，就是要提示要购买完整版，看了一下界面，发现是咪咕游戏的SDK，就想着搞事情。  

## 尝试破解

### 1. Toast提示，寻找关键字
咪咕游戏里面有支付宝支付的选项，我选择之后，点击了取消，发现了一个Toast提示，短信发送失败,请尝试其它支付方式，  

不过，当我丢入[Androidkiller](https://www.52pojie.cn/thread-319641-1-1.html)中去反编译的时候，忽然想起，签名不同，无法覆盖安装啊，无奈。

试玩到中间，会提示一个购买完整版，选择支付方式为支付宝，之后返回，提示“短信发送失败,请尝试其它支付方式”，

**关键字为短信发送失败,请尝试其它支付方式**

### 2. 破解

以此为关键字找到了两处关键代码

![](https://images2018.cnblogs.com/blog/1210268/201806/1210268-20180626214307100-2018195806.png)

但是不确定是哪一个，所以我们将其中的某一个的内容稍微修改一下，接着编译安装到手机上，之后便是可以确定为CallSMSPay$3这个smali文件，进到里面观察，可以发现没有我们想要的支付成功，撤退。，

继续寻找其他方法，想到咪咕游戏的破解方法，

- 搜索onresult

	发现一堆文件，根本就无从下手，没办法，只好一个个看下去，看完整个头都大了，撤退。

- 搜索`onbilling`

	没有结果

- 搜索支付成功

	看到了一个关键文件

	![](https://images2018.cnblogs.com/blog/1210268/201806/1210268-20180626215056787-2088433316.png)

	去看了一下它的伪java代码，发现有几个`if`语句，猜测了一下，将`bool1`赋值为`true`，然而测试的时候以失败告终。


java伪代码
```
package com.locojoy.function;

import android.widget.Toast;
import com.adobe.fre.FREContext;
import com.adobe.fre.FREFunction;
import com.adobe.fre.FREInvalidObjectException;
import com.adobe.fre.FREObject;
import com.adobe.fre.FRETypeMismatchException;
import com.adobe.fre.FREWrongThreadException;
import com.locojoy.CallBackImpl;
import com.locojoy.Util;
import com.umeng.socialize.utils.Log;

public class SMSPayResult
  implements FREFunction
{
  public FREObject call(FREContext paramFREContext, FREObject[] paramArrayOfFREObject)
  {
    bool3 = false;
    bool4 = false;
    bool5 = false;
    bool2 = false;
    str = "10000";
    try
    {
      bool1 = paramArrayOfFREObject[0].getAsBool();
      bool2 = bool1;
      bool3 = bool1;
      bool4 = bool1;
      bool5 = bool1;
      paramArrayOfFREObject = paramArrayOfFREObject[1].getAsString();
    }
    catch (FREWrongThreadException paramArrayOfFREObject)
    {
      for (;;)
      {
        bool1 = bool2;
        paramArrayOfFREObject.printStackTrace();
        paramArrayOfFREObject = str;
        continue;
        if (CallSMSPay._type == 16)
        {
          CallBackImpl.buyResult(paramFREContext, Boolean.valueOf(true));
        }
        else if (CallSMSPay._type == 30)
        {
          CallBackImpl.callBackMigu(paramFREContext, "30");
          continue;
          Toast.makeText(paramFREContext.getActivity(), "扣款失败,请尝试其它支付方式,错误码:" + paramArrayOfFREObject, 1).show();
        }
      }
    }
    catch (IllegalStateException paramArrayOfFREObject)
    {
      for (;;)
      {
        bool1 = bool3;
      }
    }
    catch (FRETypeMismatchException paramArrayOfFREObject)
    {
      for (;;)
      {
        bool1 = bool4;
      }
    }
    catch (FREInvalidObjectException paramArrayOfFREObject)
    {
      for (;;)
      {
        boolean bool1 = bool5;
      }
    }
    Log.i("ANE", "result:" + bool1 + ",code:" + paramArrayOfFREObject);
    if (bool1)
    {
      Toast.makeText(paramFREContext.getActivity(), "支付成功", 1).show();
      if (CallSMSPay._type == 1)
      {
        CallBackImpl.recoverResult(paramFREContext, "2");
        Util.stopWaiting();
        return null;
      }
    }
  }
}


```


之后，我没有放弃，继续去查了一下咪咕的官方开发文档资料，不过没有方法下手

想了想，还是回到了`CallSMSPay$3`这个`smali`文件，既然找不到支付成功的代码，那就把这个正在支付这些代码当作支付成功的代码，修改switch语句，让其一定跳转到pswitch_2这里

因为`pswitch_2`即是正在支付的那个过程

![](https://images2018.cnblogs.com/blog/1210268/201806/1210268-20180626215643342-277211437.png)

![](https://images2018.cnblogs.com/blog/1210268/201806/1210268-20180626215700090-1910073440.png)

之后测试的时候，奇迹发生了，成功了！！我的心情真的是爽了！！

不过，还没有完，咪咕游戏一般有较多的广告，这个我也得想办法破掉。

### 去除广告
首先，是退出界面的时候，会弹出两次对话框，一次是正宗的对话框，（即带有确定和取消两个选项的对话框），之后选择确定之后，它还会出现一个咪咕班的对话框，上面就是有着广告，带有两个按钮，退出游戏和返回，这时候点击退出游戏才是真正的退出游戏

我去观察了一下里面的代码，发现了四个关键文件，如下图

![](https://images2018.cnblogs.com/blog/1210268/201806/1210268-20180627083511321-1643142690.png)

通过代码，再结合Android开发中的对话框代码，`ExitGame$1`里面是点击确定按钮所要执行的代码，`ExitGame$2`则是点击取消按钮所要执行的代码

按照我们刚才的逻辑，我们点击确定之后，还会出现一个咪咕版的对话框（带有广告），这代码肯定是在`ExitGame$1`这里面.

我们进去就可以看到，下面绿色的字体就是打开咪咕对话框的关键代码，我们将这些代码注释掉，之后加上红色的那一行，这一行的原java代码就是system.exit(0)，这样点击确定按钮就可以直接退出了

```
.method public onClick(Landroid/content/DialogInterface;I)V  
    .locals 2  
    .param p1, "dialog"    # Landroid/content/DialogInterface;  
    .param p2, "which"    # I  
  
    .prologue  
    .line 45  
    const/4 v0, 0x0  
  
    invoke-static {v0}, Ljava/lang/System;->exit(I)V  #注意这里
    #new-instance v0, Lcom/locojoy/function/ExitGame$1$1;  
  
    #invoke-direct {v0, p0}, Lcom/locojoy/function/ExitGame$1$1;-><init>(Lcom/locojoy/function/ExitGame$1;)V  
  
    .line 56  
    #.local v0, "gameExitCallback":Lcn/cmgame/billing/api/GameInterface$GameExitCallback;  
   # iget-object v1, p0, Lcom/locojoy/function/ExitGame$1;->val$_activity:Landroid/app/Activity;  
  
    #invoke-static {v1, v0}, Lcn/cmgame/billing/api/GameInterface;->exit(Landroid/content/Context;Lcn/cmgame/billing/api/GameInterface$GameExitCallback;)V  
  
    #.line 57  
    return-void  
.end method
```

**PS**：经测试发现，删除了网络权限，就可以去弹窗广告，不会影响购买
