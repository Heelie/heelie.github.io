# ChatGPT初体验之账号注册

## 前言

最近几天网上到处都是ChatGPT相关的话题，弄的我也想体验一下，今天终于注册成功了账号，赶紧上手体验，顺便记录下过程

## ChatGPT介绍

先来一段ChatGPT的自我介绍吧

![chatgpt_self_introduction.png](/img/22/12/07/chatgpt_self_introduction.png)

## 注册账号

首先要注意的是整个过程全程需要爬梯子，香港的IP都不行。

前面的步骤很简单，主要是邮箱注册之后的步骤需要注意

打开官网[https://chat.openai.com/](https://chat.openai.com/)

`Sign up`->`Create an OpenAI account`->输入邮箱，Continue->输入密码，Continue

接着会提示验证邮件，我这里使用的是临时邮箱

![chatgpt_verify_email.png](/img/22/12/07/chatgpt_verify_email.png)

### 验证邮箱

打开邮箱，点击`Verify email address`

![chatgpt_verify_email_address.png](/img/22/12/07/chatgpt_verify_email_address.png)

验证之后，正常情况应该是会出现填写个人姓名的界面

![chatgpt_tell_us_about_you.png](/img/22/12/07/chatgpt_tell_us_about_you.png)

### 解决：OpenAI's services are not available in your country.
但是如果是使用的香港的ip注册的，跳转之后可能是这个样子的

![chatgpt_not_available.png](/img/22/12/07/chatgpt_not_available.png)

然后就会出现一个很奇怪的情况，无论你怎么切换梯子的地区都没用，一直是这个提示，导致无法正常注册

**解决的方法来了**

首先在浏览器手动输入
```javascript
javascript:
```
接着复制下面这段补在后面
```javascript
window.localStorage.removeItem(Object.keys(window.localStorage).find(i=>i.startsWith('@@auth0spajs')))
```

然后回车，浏览器不会有什么变化，然后再刷新页面，此时如果你的梯子地区正常，就会进入填写个人姓名的界面，下一步就是填写手机号

![chatgpt_verify_phone_number.png](/img/22/12/07/chatgpt_verify_phone_number.png)

### 使用接码平台

这一步也是必须国外手机号，试了很多免费接码平台，都不行，最后是用的[sms-activate](https://sms-activate.org/?ref=2760796)

买了个印度手机号，花了1块多人民币，直接成功，说下过程

先打开[sms-activate](https://sms-activate.org/?ref=2760796)，注册一个账号，注册过程很容易就不说了

注册完之后先看网站首页，左边的服务列表进行服务搜索：openai,点击选择OpenAI

![sms_activate_services.png](/img/22/12/07/sms_activate_services.png)
![sms_activate_service_search.png](/img/22/12/07/sms_activate_service_search.png)

选择一个国家，这里我直接选择的印度India，价格是10.5卢布

![sms_activate_service_openai.png](/img/22/12/07/sms_activate_service_openai.png)

然后右上角进行金额充值

![sms_activate_balance.png](/img/22/12/07/sms_activate_balance.png)

下滑选择支付宝支付，充值方式是美元，记入金额是卢布，直至目前发布文章，充值0.17美元，入账11.05卢布

![sms_activate_alipay.png](/img/22/12/07/sms_activate_alipay.png)

充值后右上角余额可能还是显示为，可能是系统缓存或延迟之类的原因吧，直接购买刚才选中的国家手机号即可

![sms_activate_wait_sms.png](/img/22/12/07/sms_activate_wait_sms.png)

接着回到OpenAI继续完成注册，选择对应的国家，输入购买分配的手机号，删除号码前的国际区号，India需要删除前面的91

## 注册完成
注册完成开始体验[ChatGPT](https://chat.openai.com/chat)吧！



---

> 作者: [Jay](https://github.com/Heelie)  
> URL: https://heelie.github.io/2022/221207_experience_chatgpt/  

