---
title: DeltaLake
date: 2022-08-29 14:27:59
description: Delta Lake is an open source project that enables building a [Lakehouse architecture](https://databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html) on top of [data lakes](https://databricks.com/discover/data-lakes/introduction).
tags: 
---
[Delta Lake](https://delta.io/) is an [open source project](https://github.com/delta-io/delta) that enables building a [Lakehouse architecture](https://databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html) on top of [data lakes](https://databricks.com/discover/data-lakes/introduction). Delta Lake provides [ACID transactions](https://docs.delta.io/latest/concurrency-control.html), scalable metadata handling, and unifies [streaming](https://docs.delta.io/latest/delta-streaming.html) and [batch](https://docs.delta.io/latest/delta-batch.html) data processing on top of existing data lakes, such as S3, ADLS, GCS, and HDFS.
<!--more-->

Specifically, Delta Lake offers:

- [ACID transactions](https://docs.delta.io/latest/concurrency-control.html) on Spark: Serializable isolation levels ensure that readers never see inconsistent data.

- Scalable metadata handling: Leverages Spark distributed processing power to handle all the metadata for petabyte-scale tables with billions of files at ease.

- [Streaming](https://docs.delta.io/latest/delta-streaming.html) and [batch](https://docs.delta.io/latest/delta-batch.html) unification: A table in Delta Lake is a batch table as well as a streaming source and sink. Streaming data ingest, batch historic backfill, interactive queries all just work out of the box.

- Schema enforcement: Automatically handles schema variations to prevent insertion of bad records during ingestion.

- [Time travel](https://docs.delta.io/latest/delta-batch.html#-deltatimetravel): Data versioning enables rollbacks, full historical audit trails, and reproducible machine learning experiments.

- [Upserts](https://docs.delta.io/latest/delta-update.html#-delta-merge) and [deletes](https://docs.delta.io/latest/delta-update.html#-delta-delete): Supports merge, update and delete operations to enable complex use cases like change-data-capture, slowly-changing-dimension (SCD) operations, streaming upserts, and so on.

## 业务里使用的结构化数据类型

csv

A **comma-separated values** (**CSV**) file is a delimited [text file](https://en.wikipedia.org/wiki/Text_file "Text file") that uses a [comma](https://en.wikipedia.org/wiki/Comma "Comma") to separate values. Each line of the file is a data [record](https://en.wikipedia.org/wiki/Record_(computer_science) "Record (computer science)"). Each record consists of one or more [fields](https://en.wikipedia.org/wiki/Field_(computer_science) "Field (computer science)"), separated by commas. The use of the comma as a field separator is the source of the name for this [file format](https://en.wikipedia.org/wiki/File_format "File format"). A CSV file typically stores [tabular](https://en.wikipedia.org/wiki/Table_(information) "Table (information)") data (numbers and text) in [plain text](https://en.wikipedia.org/wiki/Plain_text "Plain text"), in which case each line will have the same number of fields.

parquert

Apache Parquet是Hadoop生态圈中一种**新型列式存储格式**，它可以兼容Hadoop生态圈中大多数计算框架(Hadoop、Spark等)，被多种查询引擎支持(Hive、Impala、Drill等)，并且它是语言和平台无关的。Parquet最初是由Twitter和Cloudera(由于Impala的缘故)合作开发完成并开源，2015年5月从Apache的孵化器里毕业成为Apache顶级项目。
