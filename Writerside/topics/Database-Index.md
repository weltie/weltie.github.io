# Database-Index

## 存储类型

存储数据时，数据在硬盘[内存]中的物理顺序，分为行存，列存

![avatar](row-based-column-based.png)

### 为什么需要列存?

前提：数据相邻读取更快
`
简单来说，计算机从内存/磁盘读取/写入数据时，以数据块【例：innodb 16kb】为单位进行操作，如果数据相邻，一个数据块就可以包含目标数据，如果数据分散，则需要读取更多的数据块。
`

#### 业务类型

OLTP(On-Line Transaction Processing)：传统增删改查业务都可归为此类

OLAP(On-Line Analysis Processing): 以查为主，通常不需要改删操作，需要访问大量记录

![avatar](olap-oltp.png)

OLAP系统需要对某列大量数据进行读取，如果采用行存，目标数据不连续，会加载大量无用数据块。

## 索引

`
如果没有索引，每次查询都需要全表扫描。
`

## mysql innodb（b+tree（平衡多路查找树））

- 在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，树的叶节点保存了完整的数据记录。这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。

- 聚簇索引
    - 主键索引
      
    - ![avatar](primary-key.png)
- 非聚簇索引
    - 辅助索引
      
    - ![avatar](secondary-key.png)

- 索引流程

  辅助索引-->主键索引-->data
  or
  辅助索引-->data

- [b+tree平衡过程](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)
- `所以主键最好选择自增ID, 索引数量也不宜过多`

## elasticsearch（倒排索引）

- es 是基于 lucene 实现的分布式搜索引擎
  lucene 是一个本地的全文检索引擎库
  引擎库：用于构建引擎的工具集

- lucene数据结构

```
lucene基础概念：index, document, field, term
index: 包含一系列document
document: 是一系列field
field: 是一系列有命名的term
term: 是一个string
```

![avatar](lucene-concepts.png)

`倒排索引是基于term进行索引的`

![avatar](lucene-files.png)

`doc_values: 可选项, 列式存储`

- 索引流程

  term-index->term-dictionary->xxx->document

## clickhouse（主键稀疏索引）

- 稀疏索引：在数据主键有序, 有序, 有序的基础上，只为部分（通常是较少一部分）原始数据建立索引，从而在查询时能够圈定出大致的范围，再在范围内利用适当的查找算法找到目标数据。

- 稠密索引：所有原始数据都有索引

![avatar](clickhouse-table.png)

- 列式存储

![avatar](clickhouse-bin.png)

- 稀疏索引（主键有序）

clickhouse 为一组数据行（称为颗粒（granule））构建一个索引条目。

因为主键定义了磁盘上行的字典顺序，所以一个表只能有一个主键。

![avatar](clickhouse-indexes.png)

- 标记文件

![avatar](clickhouse-mark.png)

- 索引流程

  主键索引->标记文件->数据文件

## 参考

- mysql

<a href="https://www.kancloud.cn/kancloud/theory-of-mysql-index/41855"></a>

<a href="https://kousiknath.medium.com/data-structures-database-storage-internals-1f5ed3619d43"></a>

- elasticsearch

<a href="https://elasticsearch.cn/article/6178#tip3"></a>
<a href="https://blog.roncoo.com/article/1315839424761069569"></a>
<a href="https://lucene.apache.org/core/8_11_2/core/org/apache/lucene/codecs/lucene87/package-summary.html#package.description"></a>

- clickhouse

<a href="https://clickhouse.com/docs/zh/guides/improving-query-performance/sparse-primary-indexes/"></a>

- 其他

<a href="https://www.oracle.com/cn/database/what-is-oltp/"></a>
<a href="https://cloud.tencent.com/developer/article/1787164"></a>
