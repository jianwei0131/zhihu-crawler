知乎爬虫
====
zhihu-crawler是一个基于Java的高性能、支持免费http代理池、支持横向扩展、分布式抓取爬虫项目，主要功能是抓取知乎用户、话题、问题、答案、文章等数据，如果觉得不错，请给个star。
## 爬取结果
* 下图为爬取117w知乎用户数据的简单统计<br>
![](https://github.com/wycm/zhihu-crawler/blob/2.0/src/main/resources/img/zhihu-charts.png)
* 详细统计见 https://www.vwycm.cn/zhihu/charts

## 需要
1. jdk 1.8
2. redis
3. mongodb

## 快速开始
1. 修改```zhihu/src/main/resources/application.yaml```redis、mongodb相关配置，[application.yaml](https://github.com/wycm/zhihu-crawler/blob/3.0/zhihu/src/main/resources/application.yaml)
2. 初始化```zhihu/src/main/resources/mongo-init.sql```mongodb脚步，[mongo-init.sql](https://github.com/wycm/zhihu-crawler/blob/3.0/zhihu/src/main/resources/mongo-init.sql)
3. 设置日志路径，默认在`/var/www/logs`[logback-spring.xml](https://github.com/wycm/zhihu-crawler/blob/3.0/zhihu/src/main/resources/logback-spring.xml)
4. Run with [ZhihuCrawlerApplication.java](https://github.com/wycm/zhihu-crawler/blob/3.0/zhihu/src/main/java/com/github/wycm/zhihu/ZhihuCrawlerApplication.java )

## 特性
* 大量使用http代理，突破同一个客户端访问量限制（注：使用的都是网上公开的免费代理，近期测试来看，部分免费代理网站都做了反爬，可用的免费代理比以前少了很多，抓取速度相比以前慢了很多）。
* 支持持久化(mongodb)。
* 多线程、高性能、支持横向扩展分布式爬取。

## TODO
* 新增问题、答案、文章抓取
* 支持实时抓取，每小时更新知乎全站所有热门内容

## 更新

### 2019.02.21
* 基于Spring Boot重构项目，支持横向扩展，分布式抓取
* 数据持久化采用mongodb
* 采用基于Netty的AsyncHttpClient代替HttpClient4.5

#### 2018.07.09
* 知乎网站更新，不再需要authorization验证
* 完善单测
* 修复已知bug

#### 2017.11.05
* 知乎authorization文件更新，修改authorization获取方式。

#### 2017.05.26
* 修复代理返回错误数据，导致java.lang.reflect.UndeclaredThrowableException异常。

#### 2017.03.30
* 知乎api变更，关注列表页不能获取到关注人数，导致线程池任务不能持续下去。抓取模式切换成原来ListPageThreadPool和DetailPageThreadPool的方式。

#### 2017.01.17
* 增加代理序列化。
* 调整项目结构，大幅度提高爬取速度。不再使用ListPageThreadPool和DetailPageThreadPool的方式。直接下载关注列表页，可以直接获取到用户详细资料。

#### 2017.01.10
* 不再采用登录抓取，并移除登录抓取相关模块，模拟登录的主要逻辑代码见[ModelLogin.java](https://github.com/wycm/zhihu-crawler/blob/2.0/src/main/java/com/crawl/zhihu/ModelLogin.java)。
* 优化项目结构，加快爬取速度。采用ListPageThreadPool和DetailPageThreadPool两个线程池。ListPageThreadPool负责下载”关注用户“列表页，解析出关注用户，将关注用户的url去重，然后放到DetailPageThreadPool线程池。
DetailPageThreadPool负责下载用户详情页面，解析出用户基本信息并入库，获取该用户的"关注用户"的列表页url并放到ListPageThreadPool。

#### 2016.12.26
* 移除未使用的包，修复ConcurrentModificationException和NoSuchElementException异常问题。
* 增加游客（免登录）模式抓取。
* 增加代理抓取模块。


## 最后
* 想要爬取其它数据，如问题、答案等，完全可以在此基础上自己定制。
* 有问题的请提issue。
* 欢迎贡献代码。
* 爬虫交流群：633925314，欢迎交流。
* 需要数据(117w知乎用户基本信息资料，该数据仅供个人学习与交流使用，严禁用于商业以及不良用途)的，关注公众号即可：lwndso<br>
![一个程序员日常分享，包括但不限于爬虫、Java后端技术，欢迎关注](https://raw.githubusercontent.com/wycm/md-image/master/2019-02-28/9.png)

## 免责申明
* 本项目仅供个人学习与交流使用，严禁用于商业以及不良用途。