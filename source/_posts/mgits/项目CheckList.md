category: mgits
date: 2015-07-05
title: 项目CheckList
---

## 数据接口都尽量做成数组形式
这是为了提高拓展性,以防策划将模块提出批处理

## 代码中不要出现中文
当要出现中文的时候,可以根据个英文索引到表里查国际化文字.

服务器向客户端提示的时候发送错误号,由客户端进行国际化

## 客户端打包
在打包的时候，加上时间和svn版本号，方便渠道确定客户端的版本是一致的

## 交易类模块
交易类模块要慎重考虑,玩家可能刷元宝

## 玩家使用模拟器登录
这个要做技术处理

## id
项目里边每个模块的ID要是全区唯一的,保证合服顺利

## GWF的问题
参考[记今天域名和GWF的问题](http://blog.zhukunqian.com/?p=1377)

## 模块次数
每个模块活动都加一个每日次数限制。例如扫荡类的功能，加一个每日最大扫荡次数，避免出现角色刷货币的情况.

## 道具
* 道具信息里添加上获得来源途径信息. 将途径分类, 以区别特殊道具 

## 数据备份
每天备份玩家数据, 以便后期查bug和回档使用