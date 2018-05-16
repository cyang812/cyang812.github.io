title: 下载 tumblr 标记为喜欢的内容
date: 2018/3/3 16:54:23
tags:
- 编程
- python
- tumblr
categories:
- 教程
thumbnail: http://p7tst3obo.bkt.clouddn.com/20180303164838919?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10
---


源代码发布在github : [get_tumblr_likes](https://github.com/cyang812/get_tumblr_likes)

# 一、介绍

本项目使用 python 编写，分析 tumblr 账户中喜欢的内容，给出资源链接，并下载。
其中 `test.json` 是一份 tumblr 返回的喜欢数据的 json 示例，提取里面图片和视频的资源地址后下载，下载的内容如下图。

![这里写图片描述](http://p7tst3obo.bkt.clouddn.com/20180303164838919?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

<!-- more -->

# 二、使用方法

- 首先，你需要通过 tumblr API 来获取账户喜欢内容。这个过程是需要通过 OAuth 认证的，具体可参看[这个网页](https://www.tumblr.com/docs/en/api/v2#auth)

- 得到认证后可以通过脚本来获取资源内容，也可以通过[这个网页](https://api.tumblr.com/console/calls/user/likes#)来查询，结果会通过 json 的形式返回

- 保存你得到的 json 数据，命名为`test.json`，执行命令 `python json_parse.py`，这可以从 json 文件中提取出资源的真正链接，并存为 `url_list.txt` 文件
  ![这里写图片描述](http://p7tst3obo.bkt.clouddn.com/20180303164909831?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

- 执行 `python download.py`，之后资源文件就会挨个下载到 download 文件夹下
  ![这里写图片描述](http://p7tst3obo.bkt.clouddn.com/20180303164920621?imageView2/0/interlace/1/q/100|watermark/2/text/Y3lhbmcudGVjaA==/font/Y29uc29sYXM=/fontsize/720/fill/I0Q0RUVGMQ==/dissolve/69/gravity/SouthEast/dx/10/dy/10)

# 三、其他

- 由于众所周知的原因，tumblr 的资源地址是不能直接下载的，因此需要设置代理。测试时使用 ssr 代理本地连接，因此 `download.py` 中有 `PROXIES = { "http": "http://127.0.0.1:1080", "https": "https://127.0.0.1:1080" } `，如果是在可直接访问 tumblr 的 VPS 上运行，可对代码做如下修改。
    ```python
    # r = requests.get(url,proxies=PROXIES) # use proxy
	r = requests.get(url) 			  # directly access
    ```

- 这个项目下载的是账户中的喜欢内容，因此需要进行认证。如果是下载某个账户发布的内容，可使用[tumblr-crawler](https://github.com/dixudx/tumblr-crawler)，再次感谢 tumblr-crawler 项目