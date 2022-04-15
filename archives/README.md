## 归档文件
由于直播数据以每天更新一次（如果不摸的话）的频率无限增长，而服务器物理资源有限，所以不能在内存中缓存所有数据。本目录定期将数据库中较旧的内容导出，这些数据已经不存在于内存中，所以无法在协调服务中访问，我们把它们从内存中转移到了硬盘上。

归档文件的作用是，较旧的视频（一个月之前的），比如共计40000条弹幕，其中22222条已经被投过，如果你很喜欢这期视频，想要把剩下的弹幕补完的话，那么你需要知道过去曾经投过哪些，此时本目录下的文件会提供帮助。

由于对应数据已经不能在公开服务中访问，你需要构建自己的私人协调服务，私人协调服务搭建成功后按照正常流程使用`dmclient`投稿即可。

### 如何搭建私有协调服务

1. 安装window或linux系统。
2. 访问[https://www.python.org/downloads/](https://www.python.org/downloads/)下载一个版本不旧的Python。
3. 安装Python。
4. 使用Pip安装所有依赖，顺序为，分别在`danmuG`和`danmuG/thunder`两个文件夹下运行`pip install -r requirements.txt`命令
5. 在`danmuG/thunder`目录下创建一个服务配置文件, 名为`server_config.ini`, 一个典型的配置文件类似下述：

```ini
[secrets]
webhook_secret=webhook_secret
trust_list=["464933.xyz","*.464933.xyz"]
dev=true
host="127.0.0.1"
port=2222
logsecret="logsecret"
```

* 其中值得注意的是将dev（开发模式）设置为true，这会使得上述大多数配置项（用来对网络服务进行防御的）失效，从而降低你配置出错的概率。 *

最后通过`python dmserver.py`启动服务器，首次启动可能需要花费一些载入时间。理论上该服务器会自动根据本地文件创建一个协调数据库（所有数据为默认的未投递状态），你需要根据本文件夹中的内容将数据替换为他们真实的投递状态。之后将启动`dmclient`即可进行协调投递，同理在协调下也可以多开客户端，当然，你的客户端的选项中的协调服务器项目需要指向本地。