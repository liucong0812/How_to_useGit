#CocoaPods
##Cocoapods下载第三方过程
### 下载cocoapod的索引
在想要用cocoapods 下载第三方类的时候，首先需要先下载cocoapod的索引 

```
$sudo gem install cocoapods

```
这时间会很长，需要等待.... 等到命令执行成功后，再进行以下操作


### 搜索第三方的名字

当执行完上面的命令后，在进行搜索就会比较快，如果没有上述命令没有执行完毕，就执行搜索。那么会重新执行上述命令。

搜索命令：

```
$ pod search "三方类名称"

```

会得到：

```

CocoaPods 0.36.0.beta.2 is available.
To update use: `gem install cocoapods --pre`
[!] This is a test version we'd love you to try.

For more information see http://blog.cocoapods.org
and the CHANGELOG for this version http://git.io/BaH8pQ.

Setup completed


-> AF2OAuth1Client (0.3.6)
   AFNetworking 2 Extension for OAuth 1.0a Authentication.
   
   
    pod 'AF2OAuth1Client', '~> 0.3.6' 
   - Homepage: https://github.com/AYastrebov/AF2OAuth1Client
   - Source:   https://github.com/AYastrebov/AF2OAuth1Client.git
   - Versions: 0.3.6 [master repo]


-> AFAbstractRESTClient (1.0)
   AFAbstractRESTClient is simple abstract solution to deal with REST
   webservices using AFNetworking.
   pod 'AFAbstractRESTClient', '~> 1.0'
   - Homepage: https://github.com/burczyk/AFAbstractRESTClient
   - Source:   https://github.com/burczyk/AFAbstractRESTClient.git
   - Versions: 1.0 [master repo]


-> AFAddressBookManager (1.1)
   Get contacts from AddressBook by their phone numbers.
   pod 'AFAddressBookManager', '~> 1.1'
   - Homepage: https://github.com/Fogh/AFAddressBookManager
   - Source:   https://github.com/Fogh/AFAddressBookManager.git
   - Versions: 1.1, 1.0.1, 1.0, 0.1.1, 0.1 [master repo]

```


### 编辑 Podfile

使用你喜欢的方式打开Podfile文件，然后把上述得出的命令中 `pod 'AF2OAuth1Client', '~> 0.3.6' `编写进Podfile中

得出如下：

```
Uncomment this line to define a global platform for your project
platform :ios, '6.0'

pod 'AF2OAuth1Client', '~> 0.3.6
target 'GuruJam' do

end

target 'GuruJamTests' do

end


```

退出保存。

### 下载或者更新 ；

如果你是第一次进行下载 那么执行

```
$ pod install
```

如果你这个项目不是第一次下载第三方那个，需要执行

```
$ pod update

```
更新并不是强制这样！！！ 可以执行 `pod install`



### 在没有执行下载索引的时候下载或者更新第三方那个
如果你已经知道你想要下载的第三方的版本，可以直接添加到Podfile中

然后执行:

```
$ pod install --verbose --no-repo-update

```

或者

```
$ pod update --verbose --no-repo-update

```

这样会为你节省很多的时间！！！！

### 检查工程是否可以使用

首先试着引入第三方的头文件。

相信我 百分之八十是导不进来的 。。。

那是以为你没有修改你的头文件路径，所以你就需要添加头文件路径。

设置方法：
```
找到： Target -> Build Settings -> Search Path -> User Header Search Path
添加： ${SRCROOT}
修改：  后面的选项选为 recursive。

```

然后试试吧。。  看看能不能导入头文件了 
