# time_to_sec

## Syntax

`INT time_to_sec(DATETIME date)`

## Description

函数返回将参数time 转换为秒数的时间值，转换公式为  小时\* 3600+ 分钟\*60+ 秒

## Example

~~text
mysql> select time_to_sec('12:13:14');
+-----------------------------+
| time_to_sec('12:13:14') |
+-----------------------------+
|                        43994 |
+-----------------------------+
~~

## keyword

`TIME_TO_SEC`
