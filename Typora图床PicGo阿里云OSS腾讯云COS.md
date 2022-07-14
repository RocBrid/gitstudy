### Gitee图床被封📢📢

这段时间，平时喜欢用gitee图床写文档的同学应该都遇到了typora上传Gitee图床失败,一直报403错误的问题。在网上查了很多信息，失败的原因是Gitee仓库因为外链过多，官方加了防盗链不让用了，显然gitee 被封了。

### ❤我只能说gitee不讲武德，韭菜没了❤

既然gitee图床已经传不上去了，那就赶紧更换图床吧，毕竟本地typora的很多篇文章都寄了，及时止损，目前只有各大公司提供的OSS服务比较靠谱，本篇将教你手把手使用 阿里云的OSS服务/腾讯云的COS服务搭建图床，两种对象存储服务都很不错，大家根据实际情况具体选择一种就行。

### ❤️我们搭建图床的原因❤️

在日常使用typora写文档的时候，typora在粘贴图片之后，都是拷贝图片到本地目录，这样在分享文档给别人的时候，由于图片资源是本地的，我们每次传输文档时要么每次都附带着一个图片资源压缩包要么只能导出为pdf或者word（排版会变乱），如果我们直接发单个文件，想让别人只要需要打开typora就可以看到全部内容和图片，这个时候就要用到的图床了。

### ❤️图床的作用❤️把图片存在网站上，对应的图片生成一个链接，然后利用markdown语法插入到文本中就可以调用图片了，这样配置以后，可以提高写文档的质量和速率，轻松输出技术文档。



### ❤️Typora + PicGo +阿里云OSS图床 / 腾讯云COS图床配置思路❤️

第一步：我们首先安装PicGo，并且开放PicGo的端口号，将Typora与PicGo进行关联配置。

第二步：在完成上述操作后，购买对象存储服务（两者买其一即可），然后进行创建云存储空间并做好PicGo相关配置。

第三步：最后我们进行Typora+图片上传测试！

#### ✨安装PicGo配置Typora(非常详细)

##### 第一步：安装PicGo

- 一款功能非常强大的图床的工具，支持SM.MS、腾讯COS、GitHub图床、七牛云图床、Imgur图床、阿里云OSS、gitee等多种图床平台。

- 下载

> 下载地址：https://github.com/Molunerfinn/PicGo/releases
>
> 根据自己电脑配置进行选择安装包，一般安装PicGo-Setup-2.3.0-beta.7-ia32.exe。
>

- 特点

PicGo最大的特点是，可以和Typora结合使用，配置好关联之后，Typora写文章时，如果需要穿插图片，只需要将图片复制粘贴到Typora的编辑区域，就自动通过PicGo上传到指定图床，得到外网能访问的URL并展示。

我们在PicGo中打开PicGo设置，找到设置Server，点击设置，点击开启Server，点击确定即可。

![image-20220509161001175](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytyporaimage-20220509161001175.png)



![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytyporae33f57f82d58554651cee9893d5b8fbb.png)

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora3aa1121128f125fd35d7cf48925ab1af.png)

##### 第二步：配置Typora

我们在Typora中打开文件-偏好设置，选择PicGo(app)，路径就是安装PicGo的文件夹，选中PicGo.exe即可

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora14aa175b5c0d7a5a39e3c5c567d4edcf.png)

#### ✨云服务1：阿里云OSS搭建图床(非常详细)

##### 第一步：开通阿里云对象存储

- 开通阿里云对象存储https://www.aliyun.com/product/oss，注册阿里云账号后,开通对象储存，进入对象存储OSS的控制台。

  ![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora14aa175b5c0d7a5a39e3c5c567d4edcf.png)

##### 第二步：创建bucket

- 在左侧选择概览，然后在右侧Bucket管理中创建一个新的bucket

  ![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora6ba76278fa3c5cc4ecf372ae8a949262.png)

- 创建Bucket具体配置

> Bucket名字不能有大写字母、地域就近选择、存储类型选择标准存储，读写权限公共读
>

- 创建成功后，可以在Bucket列表中查看，记住自己的访问域名和地域节点，后面会用到。

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora6c2171b5c589100fb4752199bdfed511.png)

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora6a97a295d9ae28927bac3e334dfec1f3.png)

##### 第三步：创建AccessKey

页面右上角，鼠标放在头像处，在弹出的框里选择AccessKey管理，在弹出的选项框里，选择继续使用.进入后，创建一个AccessKey。

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora202204081630950.png)

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora202204081630455.png)



在弹出的界面里，记住你的accessKeyId和accessKeySecret。

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora82cfd2c526f3b9642fb5727ca10f1ce9.png)

##### 第四步：了解收费标准

阿里云对象存储OSS的收费是独立的，有两种收费形式

以充值的方式使用储存容量以及流量(默认状态)
按年/月收费，购买一定存储包，流量额外收费
目前我们购买了下OSS存储包，但是依旧要为访问图床的流量付钱，仔细算过以后，作为图床的数据量其实很小的，我们使用默认的0.12元/1GB/1个月，一年共计1.44元，远低于40GB的9元购买年储存包。
最后记得给阿里云账户充值几块钱，估计2块钱就可以用一年左右。

##### 第五步：配置PicGo

我们打开打开PicGo的主界面,在图床设置里面选择阿里云OSS，依照下面注意事项填写信息。

设定Keyld：填写我们在第三步中获得的AccessKeyID

设定KeySecret：填写我们在第三步中获得的AccessKeyIDSecret

设定储存空间名：填写我们在第二步中填写的bucket名称

确认存储区域：填写我们在第二步中查看的地域节点，注意复制的格式：只需要复制oss-cn-Xxxx即可，不需要后面的.aliyuncs.com

指定存储路径：其实就是自定义一个文件夹的名字，以/结尾，它会自动在你的bucket里面创建一个文件夹，并把图片上传进去。

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora3dcd2e5bbf2d3241b866deac9cee244c.png)

设置完以后，点击确定即可，后面我们统一测试。

到这里，我们阿里云OSS的图床配置完毕了😎。

### ✨云服务2：腾讯云COS搭建图床(非常详细)

##### 第一步：购买腾讯云对象存储

- 购买腾讯云对象存储 https://buy.cloud.tencent.com/cos

  ![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora9e0d3aa9079e0be37b1738c8523e3c67.png)

##### 第二步：创建桶并配置

- 相关注意信息如图所示

  ![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytypora202204081630313.png)

##### 第三步：申请密钥

- 前往https://console.cloud.tencent.com/cam/capi，我们通过配置API秘钥拿到SecretId和SecretKey。

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytyporad8e5f38357ff996376bbd954dc02bb90.png)

##### 第四步：获取其他配置信息

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytyporabbdafa34ad0f68d32cae393f1e7334e7.png)

如图，我们拿到以下信息

```
APPID：13xxxxxxx
存储空间名称：sm-13xxxxx
存储区域：ap-nanjing
```

##### 第五步：配置PicGO

根据上面配置得到的配置信息，设置PicGo的腾讯云COS参数

![img](https://typora778-1302470382.cos.ap-shanghai.myqcloud.com/mytyporamytyporab62066531cd551f74a6d91bf5e5c5686.png)

设置完以后，点击确定即可，后面我们统一测试。

到此，Typora + PicGo + 腾讯云COS 配置完毕😎。

✨Typora上传测试效果
经过上面的一系列配置之后，我们就完成了Typora的图床配置，现在我们可以使用Markdown开始写文章了，图片、截图会在粘贴之后，自动通过PicGo上传到了远端图床.

