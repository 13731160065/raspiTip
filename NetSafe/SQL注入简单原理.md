SQL注入简单原理
============================

**什么是SQL注入？**

SQL注入就是利用后台接口中的参数，配合SQL语法，猜想后台实现的代码来让后台执行一些SQL语句的解谜游戏;)。

**言归正传**

比如后台有一个接口叫做:`http://api.abc.com?name=haha`

这时就有可能有SQL注入

你可以猜想，后台代码可能是这样写的

`String sql = "select * from tabname where id='"+name+"'";`

`ResultSet res = statement.executeQuery(sql);`

以下省略......并且以下就不写大写的SQL了

别的不说，我们只看看sql部分:

`String sql = "select * from tabname where id='"+name+"'";`

如果我的`name`给他传的不是正常的`name`，而是一段sql，就实现了基本的sql注入，这就是基本原理;)

比如我传入一个:`1' or id like '%a%' #`

那么后台这个sql最后拼接成的结果为（将`name`变量替换为"`1' or id like '%a%' #`"这段语句）：

正常sql:`"select * from tabname where id='abcabc'"`

注入的sql:`"select * from tabname where id='1' or id like '%a%' #'"`

我来解释一下这个sql，前边的不必说，就是select语句，where后边的条件此时变成了如果id是1，或id模糊匹配a。我们这里可以随便弄一个id='1'，因为我们后边还有一个条件，只要让前边的条件不成立，后边条件就会执行，而后边是一个模糊查询，很容易就能成立，而后边的#（在mysql中#是注释）是将最后末尾的'注释掉，否则最后的'就会把name替换的这段代码包成一个字符串了。

把#后边注释去掉的sql语句就会变成这样:

正常sql:`select * from tabname where id='abcabc'`

注入的sql:`select * from tabname where id='1' or id like '%a%'`

这就是可怕的sql注入了，试想一下如果这是一条delete语句

`String sql = "delete * from tabname where id="+id;`

当我传入id=`123`时:

`delete * from tabname where id=123;`

这时只删除了id为123的项

当我传入id=`123123123 or 1` 时：

`delete * from tabname where id=123123123 or 1`

这时会删除id为123123123或true，如果id找到了123123123，那么123123123会被删除，如果没找到，走了后边的true，那就完了，相当于`delete * from tabname where 1`这会导致整个表被清掉！所以sql注入和接口加密还是需要重视起来的。

**如何在一个url里传入sql注入的东西呢？**

拿
