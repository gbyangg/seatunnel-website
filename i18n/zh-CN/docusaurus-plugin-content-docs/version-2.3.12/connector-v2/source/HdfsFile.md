import ChangeLog from '../changelog/connector-file-hadoop.md';

# HdfsFile

> Hdfs 文件数据源连接器

## 支持的引擎

> Spark<br/>
> Flink<br/>
> SeaTunnel Zeta<br/>

## 主要特性

- [x] [多模态](../../concept/connector-v2-features.md#多模态multimodal)

  使用二进制文件格式读取和写入任何格式的文件，例如视频、图片等。简而言之，任何文件都可以同步到目标位置。

- [x] [批处理](../../concept/connector-v2-features.md)
- [ ] [流处理](../../concept/connector-v2-features.md)
- [x] [精确一次](../../concept/connector-v2-features.md)

  在 pollNext 调用中读取分片中的所有数据。读取的分片将保存在快照中。

- [x] [列投影](../../concept/connector-v2-features.md)
- [x] [并行度](../../concept/connector-v2-features.md)
- [ ] [支持用户定义分片](../../concept/connector-v2-features.md)
- [x] 文件格式类型
  - [x] text
  - [x] csv
  - [x] parquet
  - [x] orc
  - [x] json
  - [x] excel
  - [x] xml
  - [x] binary

## 描述

从 hdfs 文件系统读取数据。

## 支持的数据源信息

| 数据源    | 支持的版本            |
|--------|------------------|
| HdfsFile   | hadoop 2.x 和 3.x |

## 数据源选项

| 名称                      | 类型    | 是否必须 | 默认值             | 描述                                                                                                                                                                                                                                                                                                                                   |
|---------------------------|---------|----------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| path                      | string  | 是      | -                   | 源文件路径。                                                                                                                                                                                                                                                                                                                         |
| file_format_type          | string  | 是      | -                   | 我们支持以下文件类型：`text` `csv` `parquet` `orc` `json` `excel` `xml` `binary`。请注意，最终文件名将以文件格式的后缀结束，文本文件的后缀是 `txt`。                                                                                                                       |
| fs.defaultFS              | string  | 是      | -                   | 以 `hdfs://` 开头的 hadoop 集群地址，例如：`hdfs://hadoopcluster`                                                                                                                                                                                                                                                                     |
| read_columns              | list    | 否       | -                   | 数据源的读取列列表，用户可以使用它来实现字段投影。支持列投影的文件类型如下所示：[text,json,csv,orc,parquet,excel,xml]。提示：如果用户想在读取 `text` `json` `csv` 文件时使用此功能，必须配置 schema 选项。                       |
| hdfs_site_path            | string  | 否       | -                   | `hdfs-site.xml` 的路径，用于加载 namenodes 的 ha 配置                                                                                                                                                                                                                                                                                       |
| delimiter/field_delimiter | string  | 否       | \001                | 字段分隔符，用于告诉连接器在读取文本文件时如何分割字段。默认 `\001`，与 hive 的默认分隔符相同                                                                                                                                                                                                                                                            |
| row_delimiter             | string  | 否    | \n                  | 行分隔符，用于告诉连接器在读取文本文件时如何分割行。默认 `\n`。                                                                                                                                                                                                 |
| parse_partition_from_path | boolean | 否       | true                | 控制是否从文件路径解析分区键和值。例如，如果您从路径 `hdfs://hadoop-cluster/tmp/seatunnel/parquet/name=tyrantlucifer/age=26` 读取文件。文件中的每条记录数据都将添加这两个字段：[name:tyrantlucifer,age:26]。提示：不要在 schema 选项中定义分区字段。            |
| date_format               | string  | 否       | yyyy-MM-dd          | 日期类型格式，用于告诉连接器如何将字符串转换为日期，支持以下格式：`yyyy-MM-dd` `yyyy.MM.dd` `yyyy/MM/dd` 默认 `yyyy-MM-dd`。日期类型格式，用于告诉连接器如何将字符串转换为日期，支持以下格式：`yyyy-MM-dd` `yyyy.MM.dd` `yyyy/MM/dd` 默认 `yyyy-MM-dd` |
| datetime_format           | string  | 否       | yyyy-MM-dd HH:mm:ss | 日期时间类型格式，用于告诉连接器如何将字符串转换为日期时间，支持以下格式：`yyyy-MM-dd HH:mm:ss` `yyyy.MM.dd HH:mm:ss` `yyyy/MM/dd HH:mm:ss` `yyyyMMddHHmmss`。默认 `yyyy-MM-dd HH:mm:ss`                                                                                                          |
| time_format               | string  | 否       | HH:mm:ss            | 时间类型格式，用于告诉连接器如何将字符串转换为时间，支持以下格式：`HH:mm:ss` `HH:mm:ss.SSS`。默认 `HH:mm:ss`                                                                                                                                                                                                                                       |
| remote_user               | string  | 否       | -                   | 用于连接到 hadoop 登录名的登录用户。它旨在用于 RPC 中的远程用户，不会有任何凭据。                                                                                                                                                                                                                                                        |
| krb5_path                 | string  | 否       | /etc/krb5.conf      | kerberos 的 krb5 路径                                                                                                                                                                                                                                                                                                                     |
| kerberos_principal        | string  | 否       | -                   | kerberos 的主体                                                                                                                                                                                                                                                                                                                     |
| kerberos_keytab_path      | string  | 否       | -                   | kerberos 的 keytab 路径                                                                                                                                                                                                                                                                                                                   |
| skip_header_row_number    | long    | 否       | 0                   | 跳过前几行，但仅适用于 txt 和 csv。例如，设置如下：`skip_header_row_number = 2`。然后 Seatunnel 将跳过源文件的前 2 行                                                                                                                                                                                                              |
| schema                    | config  | 否       | -                   | 上游数据的 schema 字段                                                                                                                                                                                                                                                                                                            |
| sheet_name                | string  | 否       | -                   | 读取工作簿的工作表，仅在 file_format 为 excel 时使用。                                                                                                                                                                                                                                                                                         |
| xml_row_tag               | string  | 否       | -                   | 指定 XML 文件中数据行的标签名称，仅在 file_format 为 xml 时使用。                                                                                                                                                                                                                                                                               |
| xml_use_attr_format       | boolean | 否       | -                   | 指定是否使用标签属性格式处理数据，仅在 file_format 为 xml 时使用。                                                                                                                                                                                                                                                                          |
| csv_use_header_line       | boolean | 否       | false               | 是否使用标题行解析文件，仅在 file_format 为 `csv` 且文件包含符合 RFC 4180 的标题行时使用                                                                                                                                                                                                                                                           |
| file_filter_pattern       | string  | 否       |                     | 过滤模式，用于过滤文件。                                                                                                                                                                                                                                                                                                               |
| filename_extension        | string  | 否       | -                   | 过滤文件扩展名，用于过滤具有特定扩展名的文件。示例：`csv` `.txt` `json` `.xml`。                                                                                                                                                                                                                                                                       |
| compress_codec            | string  | 否       | none                | 文件的压缩编解码器                                                                                                                                                                                                                                                                                                                   |
| archive_compress_codec    | string  | 否       | none                |
| encoding                  | string  | 否       | UTF-8               |                                                                                                                                                                                                                                                                                                                                              |
| null_format               | string  | 否       | -                   | 仅在 file_format_type 为 text 时使用。null_format 定义哪些字符串可以表示为 null。例如：`\N`                                                                                                                                                                                                                                           |
| binary_chunk_size         | int     | 否       | 1024                | 仅在 file_format_type 为 binary 时使用。读取二进制文件的块大小（以字节为单位）。默认为 1024 字节。较大的值可能会提高大文件的性能，但会使用更多内存。                                                                                                                                                                                                            |
| binary_complete_file_mode | boolean | 否       | false               | 仅在 file_format_type 为 binary 时使用。是否将完整文件作为单个块读取，而不是分割成块。启用时，整个文件内容将一次性读入内存。默认为 false。                                                                                                                                                                                                                   |
| common-options            |         | 否       | -                   | 数据源插件通用参数，请参阅 [数据源通用选项](../source-common-options.md) 了解详情。                                                                                                                                                                                                                                                           |
| file_filter_modified_start  | string  | 否    | -                   | 按照最后修改时间过滤文件。 要过滤的开始时间(包括改时间),时间格式是：`yyyy-MM-dd HH:mm:ss`                                                                                 |
| file_filter_modified_end    | string  | 否    | -                   | 按照最后修改时间过滤文件。 要过滤的结束时间(不包括改时间),时间格式是：`yyyy-MM-dd HH:mm:ss`                                                                                                                   |

### delimiter/field_delimiter [string]

**delimiter** 参数将在 2.3.5 版本后弃用，请使用 **field_delimiter** 代替。


### row_delimiter [string]

仅在 file_format 为 text 时需要配置。

行分隔符，用于告诉连接器如何分割行。

默认 `\n`。

### file_filter_pattern [string]

过滤模式，用于过滤文件。

该模式遵循标准正则表达式。详情请参考 https://en.wikipedia.org/wiki/Regular_expression。
以下是一些示例。

文件结构示例：
```
/data/seatunnel/20241001/report.txt
/data/seatunnel/20241007/abch202410.csv
/data/seatunnel/20241002/abcg202410.csv
/data/seatunnel/20241005/old_data.csv
/data/seatunnel/20241012/logo.png
```
匹配规则示例：

**示例 1**：*匹配所有 .txt 文件*，正则表达式：
```
/data/seatunnel/20241001/.*\.txt
```
此示例匹配的结果是：
```
/data/seatunnel/20241001/report.txt
```
**示例 2**：*匹配所有以 abc 开头的文件*，正则表达式：
```
/data/seatunnel/20241002/abc.*
```
此示例匹配的结果是：
```
/data/seatunnel/20241007/abch202410.csv
/data/seatunnel/20241002/abcg202410.csv
```
**示例 3**：*匹配所有以 abc 开头，且第四个字符是 h 或 g 的文件*，正则表达式：
```
/data/seatunnel/20241007/abc[h,g].*
```
此示例匹配的结果是：
```
/data/seatunnel/20241007/abch202410.csv
```
**示例 4**：*匹配以 202410 开头的第三级文件夹和以 .csv 结尾的文件*，正则表达式：
```
/data/seatunnel/202410\d*/.*\.csv
```
此示例匹配的结果是：
```
/data/seatunnel/20241007/abch202410.csv
/data/seatunnel/20241002/abcg202410.csv
/data/seatunnel/20241005/old_data.csv
```

### compress_codec [string]

文件的压缩编解码器及其支持的详细信息如下所示：

- txt: `lzo` `none`
- json: `lzo` `none`
- csv: `lzo` `none`
- orc/parquet:
  自动识别压缩类型，无需额外设置。

### archive_compress_codec [string]

归档文件的压缩编解码器及其支持的详细信息如下所示：

| archive_compress_codec | file_format       | archive_compress_suffix |
|------------------------|-------------------|-------------------------|
| ZIP                    | txt,json,excel,xml | .zip                    |
| TAR                    | txt,json,excel,xml | .tar                    |
| TAR_GZ                 | txt,json,excel,xml | .tar.gz                 |
| GZ                     | txt,json,excel,xml | .gz                     |
| NONE                   | all                | .*                      |

注意：gz 压缩的 excel 文件需要压缩原始文件或指定文件后缀，例如 e2e.xls ->e2e_test.xls.gz

### encoding [string]

仅在 file_format_type 为 json,text,csv,xml 时使用。
要读取的文件的编码。此参数将由 `Charset.forName(encoding)` 解析。

### binary_chunk_size [int]

仅在 file_format_type 为 binary 时使用。

读取二进制文件的块大小（以字节为单位）。默认为 1024 字节。较大的值可能会提高大文件的性能，但会使用更多内存。

### binary_complete_file_mode [boolean]

仅在 file_format_type 为 binary 时使用。

是否将完整文件作为单个块读取，而不是分割成块。启用时，整个文件内容将一次性读入内存。默认为 false。

### 提示

> 如果您使用 spark/flink，为了使用此连接器，您必须确保您的 spark/flink 集群已经集成了 hadoop。测试过的 hadoop 版本是 2.x。如果您使用 SeaTunnel Engine，则在下载和安装 SeaTunnel Engine 时会自动集成 hadoop jar。您可以检查 `${SEATUNNEL_HOME}/lib` 下的 jar 包来确认这一点。

## 任务示例

### 简单示例

> 此示例定义了一个 SeaTunnel 同步任务，从 Hdfs 读取数据并将其发送到 Hdfs。

```
# 定义运行时环境
env {
  parallelism = 1
  job.mode = "BATCH"
}

source {
  HdfsFile {
  schema {
    fields {
      name = string
      age = int
    }
  }
  path = "/apps/hive/demo/student"
  file_format_type = "json"
  fs.defaultFS = "hdfs://namenode001"
  }
  # 如果您想获取有关如何配置 seatunnel 的更多信息和查看完整的数据源插件列表，
  # 请访问 https://seatunnel.apache.org/docs/connector-v2/source
}

transform {
  # 如果您想获取有关如何配置 seatunnel 的更多信息和查看完整的转换插件列表，
    # 请访问 https://seatunnel.apache.org/docs/transform-v2
}

sink {
    HdfsFile {
      fs.defaultFS = "hdfs://hadoopcluster"
      path = "/tmp/hive/warehouse/test2"
      file_format_type = "orc"
    }
  # 如果您想获取有关如何配置 seatunnel 的更多信息和查看完整的接收器插件列表，
  # 请访问 https://seatunnel.apache.org/docs/connector-v2/sink
}
```

### 过滤文件

```hocon
env {
  parallelism = 1
  job.mode = "BATCH"
}

source {
  HdfsFile {
    path = "/apps/hive/demo/student"
    file_format_type = "json"
    fs.defaultFS = "hdfs://namenode001"
    // 文件示例 abcD2024.csv
    file_filter_pattern = "abc[DX]*.*"
  }
}

sink {
  Console {
  }
}
```

## 变更日志

<ChangeLog />