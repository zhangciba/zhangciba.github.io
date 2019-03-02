---
title: 清理Zabbix历史数据
date: 2018-02-08 18:35:41
categories: 集群管理
tags: Zabbix
keywords: Zabbix历史清理
description:
---

统计Zabbix各表的大小
```
SELECT table_name AS "Tables",
	round(((data_length + index_length) / 1024 / 1024), 2) "Size in MB" 
	FROM information_schema.TABLES
	WHERE table_schema = 'zabbix'
	ORDER BY (data_length + index_length) DESC;
```

