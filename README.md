# js-log-report
前端错误日志采集上报、上报给后端分析错误日、主要用于移动端各手机类型错误日志的收集分析





### 数据上报表结构
```
DROP TABLE IF EXISTS `j_log`;
CREATE TABLE `j_log` (
  `id` int(10) NOT NULL AUTO_INCREMENT COMMENT 'id号',
  `os_version` char(10) DEFAULT NULL COMMENT '系统版本号',
  `msg` varchar(255) DEFAULT NULL COMMENT '错误信息',
  `error_url` varchar(255) DEFAULT NULL COMMENT '错误所在的url',
  `line` int(10) DEFAULT NULL COMMENT '错误所在的行',
  `col` int(10) DEFAULT NULL COMMENT '错误所在的列',
  `error` varchar(255) DEFAULT NULL COMMENT '具体的error对象',
  `url` varchar(255) DEFAULT NULL,
  `browser` varchar(255) DEFAULT NULL COMMENT '浏览器类型',
  `product_name` char(255) CHARACTER SET utf8 DEFAULT '' COMMENT '产品名称',
  `error_time` char(20) DEFAULT NULL COMMENT '时间戳',
  `os` char(10) DEFAULT NULL COMMENT '系统类型',
  `ua` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=55 DEFAULT CHARSET=utf8;

```
