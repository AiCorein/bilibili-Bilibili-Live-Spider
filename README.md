# bilibili-Bilibili-Live-Spider
### 一、实现原理：
#### 1.核心：数据去重
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;直播间网页页面弹幕和礼物数据容量有限，且这些数据处于不断增长中，超过一定量后，将会发生滚动覆盖。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;程序每隔一段时间读取一次页面数据，若页面数据仍在容量内，则两次获取数据会有部分重复。若页面数据已经开始滚动，则两次获取数据在位置上会发生错位。因此每获取一次数据就需要进行一次去重。 **去重原理如下图：（在twice数据列表中寻找与once数据列表相同的元素，扩展once数据列表。）**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**一次去重后，once数据列表可以保留作为数据总列表，而twice数据列表可被新的一批数据覆盖，这样就可以实现重复去重**

![image](https://github.com/AiCorein/bilibili-Bilibili-Live-Spider/blob/main/img/remove_repeat.png)
#### 2.主要方法
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（1）网页数据定位方法：selenium浏览器对象访问指定直播间url，返回页面源码，再使用xpath定位对应html标签。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（2）加速去重方法：使用线程池，对弹幕和礼物列表同时去重（其实这个我不太会用(～￣▽￣)～ ）。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（3）运行时长控制：分为两种模式，运行指定时长和运行至直播间关闭。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（4）抓取监控：每进行一次抓取去重后，使用print输出一次数据列表，以实现对抓取数据的实时监控。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（5）数据抓取频率：分为两种模式，快模式和慢模式，具体可根据弹幕流量和直播间人数选择。快模式每0.5秒左右读一次页面数据，两类数据最大缓存量都为400条；慢模式每1秒左右读一次页面数据，两类数据最大缓存量都为200条。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（6）数据存储：当数据缓存量大于最大缓存量写入csv一次，并清空数据缓存，避免大量数据堆积。

## 二、使用注意事项
#### 1.使用python源码
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（1）模块依赖安装：
```shell
pip install selenium
pip install lxml
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（2）下载浏览器驱动（驱动与浏览器版本一定要对应）：参考 [爬虫利器selenium和浏览器驱动安装教程](https://blog.csdn.net/qq_44032277/article/details/105793873)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（3）修改浏览器驱动路径：浏览器驱动下载后，请修改该条源代码的路径参数为你自己的浏览器驱动路径（我的驱动路径是chromedriver）
```python
bro = webdriver.Chrome('C:/Users/15742/chromedriver.exe', chrome_options = chrome_options)
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（4）运行提示：程序在发送请求获取数据时可能因网络不稳定而报错，此时等待其快速重连即可。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（5）建议：建议在控制台运行该py源码，因为其清理输出依赖于cmd

#### 2.使用封装的exe应用程序
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;realease尚未发布，可能后续会考虑
ヽ(￣▽￣)ﾉ
