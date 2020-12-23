# READ-ME

作者：米开

微信公众号：**米开的网络私房菜** 

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/QRcode.png" alt="image-20200715095229689"  />

版权声明：本文遵循**[知识共享许可协议3.0（CC 协议）](https://creativecommons.net.cn/licenses/meet-the-licenses/)**： [署名-非商业性使用-相同方式共享 (by-nc-sa)](https://creativecommons.org/licenses/by-nc-sa/3.0/cn/) 

参考：

   《高性能Mysql》第三版

   《阿里巴巴Java开发手册》 Mysql规约部分

​	[MySQL官方文档](https://dev.mysql.com/doc) 

​	微信公众号: <SQL数据库开发>

​	[MySQL索引原理及慢查询优化](https://tech.meituan.com/2014/06/30/mysql-index.html) 

​	[如何避免回表查询？什么是索引覆盖？](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651962609&idx=1&sn=46e59691257188d33a91648640bcffa5&chksm=bd2d092d8a5a803baea59510259b28f0669dbb72b6a5e90a465205e9497e5173d13e3bb51b19&mpshare=1&scene=1&srcid=&sharer_sharetime=1564396837343&sharer_shareid=7cd5f6d8b77d171f90b241828891a85f&key=abd60b96b5d1f2e52ca45314fb2c95a67fad7a457fe265562eb51a1c026389d3f28c52359f96e920368ab44a5d08ebcbbe2ded474be2ba70731ed8b5dcc5dd68cc0eceb4989a74fb04e5055c78af8d38&ascene=1&uin=MTAwMjA4NTM0Mw%3D%3D&devicetype=Windows+7&version=62060739&lang=zh_CN&pass_ticket=tXA4xc7SZYamLpGZz5B6JwJa1ZRvZ4bRlmzFhXwEKeOfloPLulU0O80gsIQUiONb)



修改记录：

2020-02-08	米开	第一次修订	

2020-02-13	米开	增加索引相关的内容 

2020-03-10	米开	增加间隙锁相关的内容  

2020-03-12	米开	增加 redo log / undo log / bin log 相关的内容  

2020-05-10    米开	增加 binlog 和主从复制相关的知识点	

2020-06-06   米开	 修改约束相关内容，增加触发器，事件内容

2020-06-15   米开	 增加NULL值判断使用索引情况的描述

2020-07-26   米开	第一版 v0.95.0 定稿

2020-09-10   米开	完善备份、恢复章节内容

2020-09-24   米开	第二版 v0.96.0 定稿

2020-09-27   米开	增加用户-权限相关内容，状态监控

2020-10-24   米开	美化代码结构，修改排版，完善SQL基础、数据库备份方面的内容
