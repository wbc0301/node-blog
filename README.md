# node-blog
***node-blog-原生实现***

## 项目从服务到数据经过五层拆分
1. www.js 层：  创建服务，监听端口
2. app.js 层：  解析 path， query， 处理 post 请求的 data， 处理 coookie session, 引入路由， 处理404
3. router 层：  处理路由 并对路由获取到的数据进行包装（成功，失败）。通过不同的URL判断，调用不同的 controller 并且传入参数。
4. controller层：  处理数据, 通过不同的 sql 调用 `exec(sql)`，并返回结果.
5. db层： 
   1. conf/db.js 连接数据库配置文件：host,port,database,user,passsword
   2. db/blog.js,redis.js 
      - 连接 mysql: `mysql.createConnection(MYSQL_CONF).connect();` ;
      声明： `exec(sql){return new Promise(...)}` 并导出。 **因为这里返回 promise 所以 controller 层，router 层后续返回的都是 promise。**
      - 连接 redis: `redisClient = redis.createClient(REDIS_CONF.port, REDIS_CONF.host)`。
      声明：`function set(key, val){redisClient.set(key, val, redis.print)}`并导出。
      声明：`function get(key) {return new Promise(...)}` 并导出。
      
## 通过 escape 函数预防 sql 注入。
   #### 攻击方式：通过输入可执行的 sql 语句，攻击数据库。  预防原理：转义特殊字符
   ```
   const { exec, escape } = require('../db/mysql');
   
   username = escape(username);
   password = escape(password);
   ```
## xss 攻击
   #### 攻击方式： 通过在页面内容中掺杂可执行的 js 代码，以实现攻击。  预防原理：转义特殊字符
   ```
   const xss = require('xss');
   
   const title = xss(blogData.title)
   const content = xss(blogData.content)
   ```
## 密码加密
   ```
   const crypto = require('crypto');
   const SECRET_KEY = 'WJiol_8776#'; // 密匙
   
   // md5 加密
   function md5(content) {
       let md5 = crypto.createHash('md5')
       return md5.update(content).digest('hex')
   }
   ```
