# 腾讯云COS同步（批量上传）工具

## 安装

```sh
npm install -g cossync
```

## 配置

准备一个配置文件，例如`conf.json`：

```json
{
    "appId":"100012345",
    "secretId":"ABCDABCDABCDABCDABCDABCD",
    "secretKey":"abcdabcdabcd",
    "expired":1800,
    "bucket":"bucketName",
    "remotePath":"/test/",
    "localPath":"./",
    "cacheMaxAge":31536000,
    "strict":true , 
    "timeout":30,
    "mime":{
        "default": true,
        ".test": "text/plain"
    }
}
```

## params

* `remotePath` 为COS存储根目录，
* `strict` 单个文件报错是否停止上传 true or false  default : true
* `localPath` 为本地要同步的文件的根目录。`localPath`中的内容将被一一同步到`remotePath`中。
* `remotePath` 腾讯cos目录
* `cacheMaxAge` 会设置`cache-control`头为指定的`max-age`值。
* `mime` 中的`default`表示是否让cossync模块根据后缀名解析MIME（使用`mime`模块），其它键值表示需要自定义MIME。
* `timeout` 连接超时时间 s
* `progress` 查看上传进度函数
* `showLog` 打印日志，浏览器下默认不打印

## CMD使用模式

```sh
cossync conf.json
```

## require模式

```js
 'use strict';

 var Cossync = require('cossync');

 var cos = new Cossync({
     "appId":"100012345",
    "secretId":"ABCDABCDABCDABCDABCDABCD",
    "secretKey":"abcdabcdabcd",
    "expired":1800,
    "bucket":"bug",
    "cacheMaxAge":60,
    "timeout":100,
    "strict":true,
    "remotePath":"/test/",
    "progress" : function(countConf){} 
 });
//cos.async === cos.sync
 cos.sync('E:/source/2016_11/4/demo/' , {"default": true" ,.test": "text/plain"} , 60 , function(err , result){
    console.log(err , result);
 });

 //默认关闭浏览器日志打印
Cossync.setWindowsLog(false);
```

## 历史

### 2.0.0 2016-12-30

- 增加上传进度
- 增加连接超时设置
- 增加单个文件报错是否停止上传
- 兼容浏览器日志打印，完善日志打印

### 1.2.0 2016-11-08

- 增加出错时3次重试
- 出错时退出返回码改为`1`

### 1.1.0 2016-09-07

- `root`变更为`remotePath`
- 增加`localPath`选项设置本地目录
- 增加缓存头控制选项`cacheMaxAge`
- 增加MIME配置项`mime`
- 规整控制台输出

### 1.0.1 2016-09-04

- 修复命令行无法使用的问题

### 1.0.0 2016-09-04

- 基本功能实现
