# EncryptUtil
# CCPAESEncode

一行代码完成DES加密，加密模式 DES + CBC

## DEMO GIF

![Image text](https://github.com/feiyangkl/EncryptUtil/blob/master/EncryDemo/EncryDemo/Untitled.gif)

## DEMO 简介
```
最近项目中用到DES加密，在这里整理成篇，供大家参考阅读，在使用该demo过程中，你可能会遇到一些问题，首先你需要看一下下面的demo简介，看看该demo 是否适合你的项目。

项目中的DES加解密主要用在网络请求过程中对上传的参数进行加密，对从后台服务器获取的数据进行解密。

整体的加密流程为:

加密的过程: 参数字典 --> json字符串 --> base64加密后的字符串 --> DES加密后base64再加密 --> 输出最终加密后的字符串;

解密的过程： 

后台服务器获取加密的字符串 -->base64解密 --> DES解密后base64解密 --> json字符串 --> 数据字典;(与加密的过程相反)

网上对DES的详细介绍已经有很多，在这里不做赘述，如果你需要了解这些知识，google.
```

在这里感谢这些 blog 的作者,让我在开发过程中少走了很多弯路:

[http://www.open-open.com/lib/view/open1452738808948.html)

[https://my.oschina.net/jsan/blog/54385)

[http://blog.csdn.net/j_akill/article/details/44079597](http://blog.csdn.net/j_akill/article/details/44079597)

[http://blog.csdn.net/jbjwpzyl3611421/article/details/18256917)

[https://github.com/IMCCP/CCPAESEncode/blob/master/CCPAESEncode)

```
我们公司后台为JAVA,移动端有iOS与Android, 讨论后选择DES的加密模式为 DES + CBC (注意是否满足你的加密需求)。

为什么选择这种加密模式:


如果采用PKCS7Padding或者PKCS5Padding这种加密方式，末端添加的数据可能不固定，在解码后需要把末端多余的字符去掉，比较棘手。

如果不管补齐多少位，末端都是'\0',去掉的话比较容易操作。 最主要的是能使得

iOS/Android/PHP相互通信，也是加密过程中最难搞的地方，尤其需要开发者注意。


项目中用到了 google 的 base64 加解密库 GTMBase64，但是这个库已经有很多年没有更新 还是 MRC 开发模式，需要手动配置一下：

1.选择项目中的Targets，选中你所要操作的Target，

2.选Build Phases，在其中Complie Sources中选择需要ARC的文件双击，并在输入框中输入 -fno-objc-arc
```

## DEMO 使用示例
```
//加密

/// 加密
NSMutableDictionary *dic=[NSMutableDictionary dictionary];
[dic setValue:@"111111" forKey:@"password"];
[dic setValue:@"admin" forKey:@"userName"];

/*
加密{"userName":"admin","password":"111111"}和
{
"userName" : "admin",
"password" : "111111"
}
加密后结果是不一样的,一定要确定公司后台是怎么加密的,要不然有可能会错误
*/
NSString *jsonstr = [dic JSONString];
self.lb_show.text = [EncryptUtil encryptUseDES:jsonstr key:gkey];

//加密结果 iMXucxT4Z6v0ZILRJtUX1W/8KfR1wvqqdDxHiOdfTvkdVQQnJ7p1DdMPQXM60BwNHBdjhTqbnXIN
eEYVIHbb6w==

```
```
//解密

- (IBAction)clickDecodeBtn:(UIButton *)sender {

//上面加密的结果
NSString *AESString = @"iMXucxT4Z6v0ZILRJtUX1W/8KfR1wvqqdDxHiOdfTvkdVQQnJ7p1DdMPQXM60BwNHBdjhTqbnXIN
eEYVIHbb6w==";

///解密
self.lb_show.text = [EncryptUtil decryptUseDES:self.lb_show.text key:gkey];

解密结果:{"userName":"admin","password":"111111"}

}
```

项目中遇到的一些坑，在 DEMO 中都已经注释出来，写的比较清楚，如果该 DEMO 帮助了您，也希望能给个 star

鼓励一下，如果在使用中您有任何问题，可以在 github issues,我会尽自己能力给您答复 。




