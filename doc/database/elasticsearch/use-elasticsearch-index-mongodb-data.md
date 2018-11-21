# Use ElasticSearch Index MongoDB Data

使用ElasticSearch索引MongoDB的数据

## 方案

由于项目中数据量太大，直接查询很慢，建立索引开销也挺大，所以考虑搭建个搜索引擎，考虑过solr，sphinx，最终还是选择了es,原因在于mongodb到es有一个现成的中间件mongo-connector，当然solr也可以用这个


## 问答

### ElasticSearch作为NoSQL数据库(简单地存储数据)是否比在MongoDB中存储数据和使用ElasticSearch索引MongoDB具有更好的性能。

如果不需要兼容ACID的数据库，将它用作主存储是非常好的。

关于这个经常被问到的弹性小组的问题

首先是一些要点：

我们不把ElasticSearch作为主数据存储进行宣传，但是许多客户正在使用它作为主数据存储

ElasticSearch是一个最终一致的、近乎实时的存储引擎。

ElasticSearch不像数据库那样兼容ACID。

有你可以面对的问题的现状，它还取决于系统中的其他功能。

多年来，我在高流量网站上使用ElasticSearch作为主要数据存储的经验。效果很好。

## 参考

- [以mongodb为数据源搭建ElasticSearch](https://blog.csdn.net/xue632777974/article/details/78228209)
- [搭建ElasticSearch+MongoDB检索系统](https://www.cnblogs.com/jamespei/p/5694495.html)
