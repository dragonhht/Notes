# 概念

## HDFS

> Hadoop Distributed File System, 是一个分布式文件系统

1. NameNode：存储文件的元数据，如文件名、文件目录结构、文件属性，以及每个文件的块列表和块所在的 DataNode 等

2. DataNode：在本地文件系统存储文件块数据，以及块数据的检验和

### 优点

1. 高容错性

- 数据自动保存多个副本，通过增加副本的形式，提高容错性

- 某一个副本丢失后，会自动恢复

2. 适合处理大数据

- 数据规模：能够处理的数据规模达到 GB、TB，甚至 TB

- 文件规模：能够处理百万规模以上的文件数量

3. 可构建在廉价的机器上

### 缺点

1. 不适合低延时性数据访问，做不到毫秒级的存储访问
2. 无法高效的对大量小文件进行存储（NameNode 内存有限）
3. 不支持并发写入、文件随机修改

- 一个文件只能有一个写，不允许多线程同时写
- 仅支持数据的追加，不支持文件的随机写

### Shell 操作

- 命令`hadoop fs 具体命令`和`hdfs dfs 具体命令`是相同的，具体命令可查看帮助

### API

1. 导入依赖

```
implementation("org.apache.hadoop:hadoop-client:3.3.6")// 版本与hadoop版本一致
```

2. 操作示例

```java
    /**
    * 连接操作
    */
    public void hdfsOpt(Consumer<FileSystem> consumer) {
      FileSystem fileSystem = null;
      try {
          final URI uri = new URI("hdfs://hadoop01:8020");
          final Configuration configuration = new Configuration();
          String userId = "root";
          fileSystem = FileSystem.get(uri, configuration, userId);
          consumer.accept(fileSystem);
      } catch (Exception e) {
          throw new RuntimeException(e);
      } finally {
          if (fileSystem != null) {
              try {
                  fileSystem.close();
              } catch (IOException e) {
                  throw new RuntimeException(e);
              }
          }
      }
    }

    /**
     * 创建文件夹
     */
    @Test
    public void testMkdir() {
        hdfsOpt(fs -> {
            try {
                fs.mkdirs(new Path("/biancheng/java"));
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        });
    }

    /**
     * 文件上传
     */
    @Test
    public void testUpload() {
        String source = "F:\\system\\download\\recommended.yaml";
        String target = "/biancheng/java";
        hdfsOpt(fs -> {
            try {
                fs.copyFromLocalFile(false, true, new Path(source), new Path(target));
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        });
    }
```

## YARN

> Yet Another Resource Negotiator,另一种资源协调者，是 Hadoop 的资源管理器

1. ResourceManager: 管理整个集群的资源(内存、CPU 等)
2. NodeManager: 管理单个服务器的资源(内存、CPU 等)
3. ApplicationMaster: 管理单个任务的运行
4. Container: 容器，封装了任务运行所需的资源，如内存、CPU、磁盘、网络等

## MapReduce

> 负责计算，MapReduce 将计算过程分为 Map 和 Reduce

1. Map: Map 阶段并行处理输入数据
2. Reduce：Reduce 阶段对 Map 结果进行汇总
