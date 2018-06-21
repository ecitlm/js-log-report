# js-log-report
前端错误日志采集上报、上报给后端分析错误日、主要用于移动端各手机类型错误日志的收集分析


### 业务背景
> 主要用于移动端各手机类型错误日志的收集分析

### 为何要做错误日志追踪上报
前端JS代码错误，浏览器都都会在控制台输出错误信息，以及出错的文件，行号，堆栈信息，这些错误很容易导致页面代码不执行，并且考虑到手机类型五花八门，浏览器内核以及版本的差异性，前端代码机型兼容性问题，并不能将所有的手机都拿来适配，前端错误日志上报是一个较好的解决方案


### 日志上报哪些数据
1.通过 `wiindow.onerror` 可以获取 `msg, url, line, col, error`等错误信息,JS 的错误行号、url错误地址，
2.通过 `window.navigator.userAgent` 获取 设备浏览器的信息集合
如：
```
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1
```
3. `os_version` 系统版本号
4. `browser` 浏览器类型 `Opera` `FF` `Chrome` `Safari` `IE`
```javascript
 var defaults = {
    ua: window.navigator.userAgent,
    browser: '',
    os: '',
    osVersion: '',
    errUrl: window.location.href,
    msg: '', // 错误的具体信息
    url: '', // 错误所在的url
    line: '', // 错误所在的行
    col: '', // 错误所在的列
    error: '' // 具体的error对象
  }
```
具体上报字段可查看表结构



### 如何实现错误上报
1.实现错误日志收集 收集onerror 错误参数，以及自定义的参数
2.核心`window.onerror`，重写该方法、在此中捕获异常错误信息、并且将错误信息发送至服务器接口
大致代码如下
```javascript
window.onerror = function (msg, url, line, col, error) {
  ajax({
    url: 'xxx/api/sendError', // 请求地址
    type: 'POST', // 请求方式
    data: data, // 请求参数
    dataType: 'json',
    success: function (response, xml) {
      // success
    },
    fail: function (status) {
      // error
    }
  })
}
```



### 如何使用
> 使用如`index.html`所示,导入以下代码在页面head中，并且优先于其他JS文件加载

```html
<script type="text/javascript" src="./error.js"></script>
    <script type="text/javascript">
      var data = {
      productname: 'test' //产品名称
      extend:'' //扩展字段,允许是JSON 字符串的形式保存
      }
      errLogReport({
      data: data,
      url: 'http://localhost:8080/api/sendError'
      })
    </script>
```

### 数据上报表结构
```mysql
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
  `extend` varchar(255) DEFAULT NULL COMMENT '业务扩展字段、保存JSON字符串',
  `ua` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=MyISAM AUTO_INCREMENT=55 DEFAULT CHARSET=utf8;

```

### 源代码
[GitHub](https://github.com/ecitlm/js-log-report)

### 缺点
对于压缩的代码，报错信息没法定位到具体是什么地方报错了，这里没有去详细研究，阮一峰老师有篇相关文章 
[JavaScript Source Map 详解](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html)，有时间再去研究一下

