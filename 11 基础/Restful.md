**以资源为基础**
**统一接口**

- GET（SELECT）：从服务器取出资源（一项或多项）。
- POST（CREATE）：在服务器新建一个资源。
- PUT（UPDATE）：在服务器更新资源（客户端提供完整资源数据）。
- PATCH（UPDATE）：在服务器更新资源（客户端提供需要修改的资源数据）。
- DELETE（DELETE）：从服务器删除资源。

**URI指向资源**
**无状态**

**URL设计规范**
URI = scheme "://" host ":" port "/" path [ "?" query ][ "#" fragment ]
path: 访问资源的路径，就是各种web 框架中定义的route路由
query: 查询字符串，为发送给服务器的参数，在这里更多发送数据分页、排序等参数。
fragment: 锚点，定位到页面的资源

通常一个RESTful API的path组成如下：
/{version}/{resources}/{resource_id}
resources：资源，RESTful API推荐用小写英文单词的复数形式。
resource_id：资源的id，访问或操作该资源。

当然，有时候可能资源级别较大，其下还可细分很多子资源也可以灵活设计URL的path，例如：
/{version}/{resources}/{resource_id}/{subresources}/{subresource_id}

此外，有时可能增删改查无法满足业务要求，可以在URL末尾加上action，action就是对资源的操作，例如
/{version}/{resources}/{resource_id}/action

对于RESTful API的URL具体设计的规范如下：
不用大写字母，所有单词使用英文且小写。
连字符用中杠"-"而不用下杠"_"
正确使用 "/"表示层级关系,URL的层级不要过深，并且越靠前的层级应该相对越稳定
结尾不要包含正斜杠分隔符"/"
URL中不出现动词，用请求方式表示动作
资源表示用复数不要用单数
不要使用文件扩展名

获取dog/dogs/query/{dogid}GET： /dogs/{dogid}
插入dog/dogs/addPOST： /dogs
更新dog/dogs/update/{dogid}PUT：/dogs/{dogid}
删除dog/dods/delete/{dogid}DELETE：/dogs/{dogid}

