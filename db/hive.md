> ## Hive是什么? 跟数据库区别?
> hive 是用于大数据分析处理的工具，存储基于 HDFS,计算基于 MapReduce 或 Spark，提供类 SQL 查询。
> hive 除了可以通过类 SQL 查询这一点和数据库有点关系外，其它基本没啥关联。

> 数据库支持事务，可读可写；而hive一般不支持事务（高版本除外），一般用于读多写少的情况，不建议改动数据，因为数据存储在HDFS中，而HDFS的文件不支持修改；
> 
> hive延迟比较大，因其底层是MapReduce，执行效率较慢。但当数据规模较大的情况下，hive的并行计算优势就体现出来了，数据库的效率就不如hive了；
> 
> hive不支持索引，查询的时候是全表扫描，这也是其延迟大的原因之一。
>
> ## Hive的优点
> 极低的学习与迁移成本 (SQL-on-Hadoop):</br> 
> Hive 定义了一种类 SQL 的查询语言 —— HQL (Hive Query Language)。让不懂 Java/Scala 写不出 MapReduce 代码的分析师，也能通过简单的 SELECT 语句处理 PB 级数据。它可以无缝对接 BI 工具（如 Tableau、Superset）以>及各种 Java JDBC 连接，这对于你构建后端数据服务非常方便。
>
> 强大的可扩展性 (Scalability): </br> 
> Hive 运行在 Hadoop 集群之上，继承了其天然的扩展能力.依赖 HDFS，数据量从 GB 增长到 PB，只需要增加廉价的硬盘节点。依赖 YARN 资源调度，可以通过增加服务器节点来线性提升计算能力。
>
>
> 丰富的函数库与扩展能力: </br> 
> 拥有极其丰富的内置聚合函数、窗口函数（OLAP 必>备）以及UDF (User Defined Functions)
>
> 支持多种存储格式与压缩(冷库, 存储便宜): </br> 
>Hive 支持多种针对大数据优化的列式存储格式，如 ORC 和 >Parquet。结合 Snappy 或 Zstd 压缩，可以极大地节省磁盘空间，并利用“谓词下推”显著提升查询性能。

> ## 从ck把数据每日同步到一张hive表, 有什么优点?
> 极大地降低海量历史数据的存储成本</br> 
> ClickHouse 的磁盘占用通常高于 HDFS，且磁盘扩容比 Hive 昂贵得多。 Hive 使用 ORC/Parquet 格式配合 Snappy/Zstd 压缩，对于海量历史数据，存储密度远高于 ClickHouse，这能显著降低公司的数据存储费用。
>
> 构建数仓的“数据血缘”与“溯源”</br>
> ClickHouse 往往是面向业务的（面向查询），而 Hive 才是标准的数仓底座（Data Warehouse）。 通过每日同步，你可以将 ClickHouse 中分散的业务指标，按照数仓标准（ODS -> DWD -> DWS）在 Hive 中进行重新分层、清洗和建模。
>
>
> 解耦实时业务压力与离线计算</br>
>  将数据同步到 Hive 后，离线报表和复杂的模型训练任务是在 Hadoop 集群上执行的，完全不影响线上 ClickHouse 的实时查询性能。
>
> 弥补 ClickHouse 不擅长的“超级关联” (Large-scale Joins)
Join 瓶颈</br>
> ClickHouse 虽然快，但极其惧怕大表与大表之间的 Join（尤其是 Shuffle Join） Hive 配合 Spark 引擎，在处理超大规模数据的 Shuffle、SortMergeJoin 方面有极其成熟的算法（如 Broadcast Join, Bucketed Join）。对于这种需要跨多表、跨多业务线的复杂分析，Hive 的离线处理能力更强。
