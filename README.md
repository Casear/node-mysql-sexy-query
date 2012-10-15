mysql-sexy-query
====================
>  一个优雅的 sql 构造器 


### 新版特性 ：

 * 用 CoffeeScript 重构代码
 * 降偶，使用`适配器`的方式与驱动连接
   * 支持 [node-mysql](https://github.com/felixge/node-mysql)
   * 支持 [mysql-native](https://github.com/sidorares/nodejs-mysql-native)
   
   
### DEMO ：
``` CoffeeScript

### 
  使用 node-mysql 适合器 ,
  注： 如使用 mysql-native ，代码为：
  mysql = require('node-mysql-sexy-query').mysql_native
###
mysql = require('node-mysql-sexy-query').mysql
    
connection = mysql.create_connection(
  host     : 'localhost',
  user     : 'root',
  password : '',
  database : 'test'
)

class User extends mysql
    table_name : 'user'
    connection : connection

class Role extends mysql
    table_name : 'role'
    connection : connection
    
# 查询 id 为 1 的用户
User.find('id = ?' , 1).execute rows ->
    console.log rows
    
# 'order' and 'get'
User.find().order('id DESC').get(10 , (rows) ->
    console.log rows
) 

# 查询所有 sex 为 0 的用户
User.find('sex = ?' , 0).all (rows) ->
    console.log rows 
    
# 统计 count
User.find('sex = ?' , 0).count (count) ->
    console.log count
    
# 分页
ar = User.find().order('id DESC').page(1,10) 
ar.execute (rows) ->
    console.log rows
# 分页信息
ar.get_pagination (pagination) ->
    console.log pagination 
    
# 关联查询 join
User.as('U').join(Role.as('R) , '@.role_id = R.id')
       .select('@.id , @.name , R.name AS role)
       .page(1,10)
       .execute (rows) ->
           console.log rows
    
```