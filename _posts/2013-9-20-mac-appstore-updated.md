---
layout:  post
title:  "Mac系统AppStore不能更新之解决方案"
category: ut
tags:  [Mac,AppStore,Goagent,AppID]

---
{% include JB/setup %}

###背景
   我的Macbook Pro好久没有提示更新了，今天进入App Store发现工具栏中的已购项目和更新两项怎么点击都没有反应，显示无法连接，或者是什么都不显示，空白；本来点击哪个按钮哪个变成蓝色，前一次点击那个按钮此时应该变成正常的灰色。现在可好，几个按钮可以同时变蓝色，不能复位。上网查了查，大概都是在说中国区的App Store服务器有问题，经常不能正常访问。我表示无语。算了，中国区不能上，那我就上苹果老巢-美国区的App Store服务器。还别说，老巢的就是不错，一切使用正常。不过要完成这个工程，需要以下两个个步骤。
* 首先，我们国家有长城，所以要浏览国外网页，下载东西，都是需要Cross Wall的，为了要能连接上美国区App Store服务器，我们需要Goagent这个代理工具。
* 其次，苹果恶心的不许账号通用，在哪个区注册的账号只能在那个区使用，所以我们需要注册一个美国区的APPID。  
下面就让我们一步步来完成这两个步骤。  
####Goagent安装和配置  
#####1、创建Google App Engine账号  
利用GoAgent翻墙的第一步就是要申请自己的GAE账号。往往很多人就卡在这一步，然后放弃了。其实并不难！   

Step 1 -申请Google App Engine账号   
登录<http://appengine.google.com>，如果你已经拥有一个Gmail账户，直接输入账号密码就可以登录；如果没有则需要新申请一个Gmail账户。（笔者认为Gmail是必用的邮箱之一，木有的童鞋赶快申请一个）

Step 2 -创建Application  
![photo](/img/2013-09-20-goagent-install/gi1.jpg)

Step 3 -通过短信验证你的账户  
![photo](/img/2013-09-20-goagent-install/gi2.jpg)  
需要短信验证才可以进行下一步操作，Country and Carrier（国家和运营商）处选择Other，Mobile Number（手机号码）处填写你的个人手机号码号码，格式为+8613912345678。

Step 4 -将手机收到的验证码输入并Send  
![photo](/img/2013-09-20-goagent-install/gi3.jpg)  
你将会收到谷歌发给你的短信，短信内容大致为：Google App Engine:六位数字。

Step 5 -创建一个属于你的Application  
![photo](/img/2013-09-20-goagent-install/gi4.jpg)  
1）输入一个Application ID，允许使用英文字母和短横杆；  
2）点击Check Available，检查一下是否可用；  
3）输入Application名称，这里可以随便填；  
4）勾选I accept these terms，即接受协议；  
5）最后Creat Application  

Step Final -当你看到以下画面，说明你已经成功创建一个 Application  
![photo](/img/2013-09-20-goagent-install/gi5.jpg)  
**注**：每个Gmail账户最多只能够创建10个Google App Engine应用，每个应用每天有1GB免费流量。如果你经常下载或者观看视频，建议多创建几个Google App Engine应用。

####2、修改谷歌账号两步验证  
Step 1 -进入谷歌账户设置页面<https://www.google.com/settings>，在安全性-两步认证处，点击修改；  
![photo](/img/2013-09-20-google-change/gc1.jpg)

Step 2 -开始设置Google账户；  
![photo](/img/2013-09-20-google-change/gc2.jpg)

Step 3 -手机设置，此处你需要点击发送验证码，获取验证码后提交确认进入下一步；  
![photo](/img/2013-09-20-google-change/gc3.jpg)

Step 4 -验证计算机，如果当前计算机不是你个人的计算机，不要勾选信任此计算机；  
![photo](/img/2013-09-20-google-change/gc4.jpg)

Step 5 -设置两步验证的最后一步，激活  
![photo](/img/2013-09-20-google-change/gc5.jpg)

Step 6 -开始为你的Application创建密码  
![photo](/img/2013-09-20-google-change/gc6.jpg)

Step 7 -输入名称，这里可以随便填写，点击生成密码进入下一步；  
![photo](/img/2013-09-20-google-change/gc7.jpg)
  
Step 8 -把生成的密码记下来，后面会用到的。  
![photo](/img/2013-09-20-google-change/gc8.jpg)

####3、配置GAE程序    
Step 1 -下载goagent客户端  
[下载地址1](http://code.google.com/p/goagent/)   
[下载地址2](http://goagent.com.cn/?p=102)；   

Step 2 -将下载后的文件解压缩，复制到用户名的家目录下或者直接放到应用程序文件夹中，图示中是放在家目录。   
![photo](/img/2013-09-20-goagent-set/gt1.jpg)

step 3 -修改`goagent/local`文件夹中的`proxy.ini`文件，将appid修改成你的appid，即前面创建创建Application所设定的Application ID，如果是多个中间用`｜`隔开，例如：ppnna|ppnnb   
![photo](/img/2013-09-20-goagent-set/gt2.jpg)

step 4 -上传Goagent文件到Google app 打开终端输入 `cd goagent/server` 回车，切换到server目录,输入`python uploader.zip` 回车，根据提示依次输入Application ID，邮箱地址，和修改谷歌账号两步验证Step 8中生成的16位密码。（注，输入密码时，文字是不可见的，确定输入后回车确认即可。）命令行最后出现Completed update of app…说明已经上传成功。
![photo](/img/2013-09-20-goagent-set/gt3.jpg)
![photo](/img/2013-09-20-goagent-set/gt4.jpg)

####设置Goagent全局代理（局部的浏览器代理会出现错误）
step 1 -创建网络位置
创建步骤：系统偏好设置-网络-位置（默认是自动）-编辑位置-新建网络位置（我这里命名为代理）-完成。
![photo](/img/2013-09-20-goagent-use/gu1.jpg)

Step 2 -设置代理
只勾选自动代理配置，URL该填的是`.pac`文件的位置，用选取文件去找到`~/goagent/local/proxy.pac`。将`swscan.apple.com、swquery.apple.com、swdownload.apple.com、swcdn.apple.com`加到忽略这些主机与域的代理设置中。
![photo](/img/2013-09-20-goagent-use/gu2.jpg)

Step 3 -终端命令行激活
在终端输入`cd goagent/local` 回车，然后再输入`python3 proxy.py` 回车。看到如下界面后，就可以最小化终端窗口，自由了。只要保持终端窗口不关闭，代理就一直运行，不想用的时候关闭终端程序，切换回正常的网络位置（自动）即可。   
![photo](/img/2013-09-20-goagent-use/gu3.jpg)

###申请美国区Apple ID

#####美国区Apple ID的好处：
1、美国区AppStore服务器稳定。
2、在美国区的AppStore商店里，有许多内容包括：歌曲、视频、Podcast在中国区是没有的。2.几乎所有的应用都会出现在美国区AppStore里，部分应用在中国区是没有的。  
3、很多通过做任务赚取积分来兑换免费iTunes或者Amazon现金卡的应用都需要你拥有一个美国区的Apple ID。

#####注册准备：
1、先找到一个美国的VPN代理，这里我们使用goagent，用goagent的话，在IE的设置里启用goagent的话，iTunes也会使用与IE一样的代理设置。如果不用美国代理，在提交申请的时候，你会得到please contact iTunes support to complete this transaction的错误提示。  

2、找到一个虚拟的美国身份，最好用美国5个免税州的身份，这样以后有充值的话，购买东西也可以免税。5个免税州分别是DE-Delaware(特拉华)，OR-Oregon(俄勒冈)，MT-Montana(蒙大那)，AK-Alaska(阿拉斯加)，NH-New Hampshire(新罕布什尔)，你也可以按照以下填写:
名字：Lena，姓氏：Hampton，地址：1466 El Camino Dr，城市：Florissant，州缩写：MO，邮编：63033，区号：314，电话：838-8350

#####开始申请Apple ID  

1、首先在你电脑上安装iTunes，安装完后打开，如果有登陆过中国区的Apple ID请先注销，然后点击前往iTunes Store。在iTunes Store界面拉到最下面，点击更改国家或地区，切换到美国区。  

![photo](/img/2013-09-20-appid/app1.jpg)

2、在右侧边栏，找到TOP FREE APPS，随便点击一个免费的应用，然后选择安装，会弹出一个登陆的窗口。  

![photo](/img/2013-09-20-appid/app2.jpg)

3、在登陆窗口上点击“创建 Apple ID”，在下个页面点击“Continue”，之后是用户协议，打钩，点击“Agree”，开始填写账号信息。你需要输入你的电子邮件地址、创建密码、创建安全问题和答案，然后输入你的生日。点击“Continue”  
![photo](/img/2013-09-20-appid/app3.jpg)  
![photo](/img/2013-09-20-appid/app4.jpg)

4.**如果你不是通过点击免费应用开始注册账号的话，到这一步的信用卡选项里是没有None的**，其他按照下图标示填写。  
![photo](/img/2013-09-20-appid/app5.jpg)

5.之后便会提示你到邮箱中确认，打开你的邮箱点击确认链接，会要求你填写账号密码，之后便完成申请了。如果登陆失败，请重新回到iTunes注册页面，点击重新发送确认邮件。

OK!到这里一切都结束了，自由的下载更新吧！

