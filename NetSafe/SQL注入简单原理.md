SQL注入简单原理
============================

**什么是SQL注入？

SQL注入就是利用后台接口中的参数，配合SQL语法，猜想后台实现的代码来让后台执行一些SQL语句的解谜游戏;)。

**言归正传

比如后台有一个接口叫做:http://api.abc.com?name=haha

这时就有可能有SQL注入

你可以猜想，后台代码可能是这样写的

String sql = "select * from tabname where id='"+name+"'";

ResultSet res = statement.executeQuery(sql);

以下省略......并且以下就不写大写的SQL了

别的不说，我们只看看sql部分，
