---
title: Spark streaming fails with NativeAzureFileSystem on HDP 2.6
description: Suggested to change to fs.azure.flatlist.enable
services: spark-streaming
author: Sunilkc
ms.reviewer: 
ms.service: HDInsight
ms.topic: conceptual
ms.date: 05/31/2018
---

## Symptom : Hive jobs Performance degraded, identified millions of files in temporary directory of Hive external table.

#### Issue:
Stream job was throwing error shown below.
```
2018-03-22 18:40:16 WARN [AzureFileSystemThreadPoolExecutor.java] org.apache.hadoop.fs.azure.AzureFileSystemThreadPoolExecutor: Failed to Delete file FileMetadata[data/kcnet_datalake/event_store/version=v2.0/_temporary/0, 0, 0, ::rwxrwxrwx] 
2018-03-22 18:40:16 ERROR [NativeAzureFileSystem.java] org.apache.hadoop.fs.azure.NativeAzureFileSystem: Failed to delete files / subfolders in blob data/kcnet_datalake/event_store/version=v2.0/_temporary 
```

#### Resolution 
We have identified the issue is already detailed [here](https://issues.apache.org/jira/browse/HADOOP-14769) . But this jira patch mentioned is fixed in hadoop 2.9. As of 05/31/2018 HDInsight is using HDP 2.6.2.25-1 [AzureNativeFileSystemStore]((https://github.com/hortonworks/hadoop-release/blob/HDP-2.6.2.25-tag/hadoop-tools/hadoop-azure/src/main/java/org/apache/hadoop/fs/azure/AzureNativeFileSystemStore.java))

#### Mitigation:
 Set ``` fs.azure.flatlist.enable=true ``` in ``` core-site.xml ```  for HDFS and restart the service
