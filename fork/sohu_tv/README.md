问题1: 爬取tv.sohu.com的页面, 提取视频相关信息，不可用爬虫框架完成

需求:

做到最大可能的页面覆盖率
选择重要的信息进行存储
选择合适的数据存储方式，便于后续使用
可通过参数限制要抓取视频信息的数目
要用多线程方式完成抓取
反防抓取策略
*分布式支持
*崩溃后进度恢复
星号部分为加分项, 可只给出设计思路 ...




广度优先，从一个入口开始找到所有链接，将链接加入队列，将队列中的地址取出，再遍历查找地址，重复操作，知道寻找到所有链接
为了避免重复，可以使用MongoDB存储所有已经找到的链接，对于新遍历到的链接与数据库中链接对比，如果链接已存在则弃掉，不存在则加入数据库，并加入队列



数据存储在MongoDB，为提高性能对于已经抓取的url可以用redis存储

分布式可以利用MongoDB的分片？

崩溃后的恢复？
设置连接超时，在第一次超时后重新尝试链接？
所有已经遍历的地址存储在数据库中在一个地址被成功处理后可以做一条标记，在崩溃后重新从未标记的位置开始搜索。



### 对于视频页面的分析

div class=infoBox cfix  id=info
<div class="info info-con">
<div class="area cfix" id="content">



页面body部分信息不够统一，重要信息在head中已经包含
页面信息直接从head获取

从http://tv.sohu.com 或 http://tv.sohu.com/map 开始抓取，似乎没有大的区别，


只抓取单独的视频页，不抓取分类和剧集等页面信息

官方发布的视频：http://tv.sohu.com/20140304/n395970803.shtml
日期+编号   n?

用户上传的视频：http://my.tv.sohu.com/us/200430155/63582287.shtml
用户名+编号