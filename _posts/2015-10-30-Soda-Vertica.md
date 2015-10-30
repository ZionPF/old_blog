---
layout: post
title: "SODA瓶之: Vertica on OpenStack"
description: ""
category: tech
tags : [intro, tutorial, cloud, bigdata]
---

编辑/整理：张鹏飞
- - -

#关于SODA瓶


此次SODA比赛，我们为进入复赛的参赛队伍免费提供了两个数据平台，
并分别导入大赛数据，供选手使用。它们分别是：

+ Spark
+ Vertica

本文介绍 Vertica 环境的部署过程、使用方法以及针对大赛数据集的压力测试。

** SODA瓶之Spark环境请戳
** SODA瓶之Ganglia监控请戳

#HP Vertica介绍


Vertica，全称 Vertica Analytic Database，是HP公司的基于DBMS架构的数据库系统，适合读密集的分析型数据库应用，比如数据仓库。从命名中也可以看到，Vertica代表它数据存储是列式的，Analytic代表适合分析型需求，DB代表本身是数据库，支持SQL。

对应比赛中的需求，我们看中的Vertica优势主要有：

* 列式存储和计算
* 大规模并行处理 (MPP)
* 数据库内部分析库
* 对于 MapReduce 和 Hadoop 的原生支持
* 标准SQL接口

这里给出Vertica的相关资料和手册：

[Vertica 官网](https://www.vertica.com/)
[Vertica 在线文档](http://my.vertica.com/docs/7.1.x/HTML/index.htm)
[Vertica SQL手册](http://my.vertica.com/docs/7.1.x/PDF/HP_Vertica_7.1.x_SQL_Reference_Manual.pdf)
[Vertica 驱动](http://www.vertica.com/resources/vertica-client-drivers/)

#Vertica on OpenStack 部署

为管理和扩展方便，我们将Vertica部署到生产环境的OpenStack云中。

###环境配置

* 3台虚拟服务器,配置：8core cpu，64GB内存，6TB硬盘空间
* 操作系统：Cent OS 6.x
* 私有虚拟网络，单一虚拟路由器
* 物理服务器之间为万兆网络
* 域名地址: vertica.soda.sjtu.edu.cn 

###Vertica部署及数据导入

OpenStack上的环境准备完成后，Vertica软件的部署由HP的工程师远程协助完成，在此感谢HP的大力协助！

数据导入方面，我们对于原始数据进行初步的整理之后创建每个表的DDL（数据定义语言），之后通过脚本调用VSQL语句导入数据库。DDL需要指定表的名称、列及列的数据格式，以一个较大的表（出租车信息）为例，其格式如下：

    DROP SCHEMA sdata CASCADE;
    CREATE SCHEMA IF NOT EXISTS sdata; 
    DROP TABLE sdata.qs_taxi CASCADE;  
    CREATE TABLE sdata.qs_taxi
    (
        id              INT 
        ,bj_flag        CHAR(1)       
        ,kc_flag        CHAR(1)          
        ,dd_flag        CHAR(1)             
        ,gj_flag        CHAR(1)             
        ,sc_flag        CHAR(1)             
        ,js_time        TIMESTAMP           
        ,gps_time       TIMESTAMP           
        ,jd             DECIMAL(16,6)       
        ,wd             DECIMAL(16,6)       
        ,sd             DECIMAL(16,1)       
        ,fx             DECIMAL(16,1)       
        ,wx             INT                 
    )
    SEGMENTED BY HASH (id) ALL NODES KSAFE;                          
    COMMENT ON TABLE sdata.qs_taxi IS '出租车信息'; 

对于数据文件，通过一个脚本利用上述DDL批量导入：

    fileDir= \*\*\*\*\*/强生出租车行车数据
    DirList=`ls ${fileDir}`
    tableName=sdata.qs_taxi

    for i in ${DirList}
    do
        $VSQL <<-EOF 2>&1 | grep -vw "WARNING"  
            \timing
            copy ${tableName} from '${fileDir}/${i}/part*' on v_sodadb_node0001
            DELIMITER ',' NULL AS '' NO ESCAPE DIRECT;
        EOF
    done


#Vertica 的性能测试

作为大赛平台，我们在Vertica上为所有参赛队伍创建了相应的帐号，但由此对Vertica的多用户并发请求响应产生了要求。因此在上线之前，我们对于这个平台和数据集进行了一些简单的压力测试。

Vertica对于参赛选手的友好体现在支持标准的SQL，可以通过安装驱动后命令行或相关GUI软件（如 DBVisualizer等）调用SQL直接操作。在测试中，我们通过脚本调用VSQL的语句进行表的一些简单查询和匹配，通过模拟多用户调用得到查询的响应时间。

对于上例中出租车表，33亿条记录的全表Count查询，我们做了最多50用户并发的查询时间测试：

![Vertica_multiuser](https://help.github.com/assets/help/footer-logo-56d3698f3654d6403360623c353d37ea.png "GitHub,Social Coding")

可以看到，查询时间随着用户增加基本上是线性增长的趋势，可以满足大赛规模的用户请求。

另一方面，Vertica对于数据进行了主动压缩，还是上面这个表为例，原始数据280GB被压缩到了30+GB，可以比较轻松的放入内存中进行查询（每个节点64GB内存），因此效率上比较有保证。

#Vertica的监控

Vertica自带一个很全面的WebUI监控界面，包括了数据库、节点、用户、查询等的历史信息。而为了保证物理资源的稳定，我们在OpenStack环境及Vertica虚拟机中安装了Ganglia的gmond，对于各类资源进行实时的监控。

关于监控系统的部署、参数以及结果，敬请点击

[SODA瓶之Ganglia监控]()












