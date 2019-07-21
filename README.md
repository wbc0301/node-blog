# node-blog
***node-blog-原生实现***

## 项目从服务到数据经过五层拆分
1. www.js 层：  创建服务，监听端口
2. app.js 层：  解析 path， query， 处理 post 请求的 data， 引入路由， 处理404
3. router 层：  处理路由 并对路由获取到的数据进行包装（成功，失败）。
4. controller层：  处理数据, 调用 `exec(sql)`
5. db层： 
   1. conf/db.js 连接数据库配置文件：host,port,database,user,passsword
   2. db/blog.js,redis.js 
      - 连接 mysql: `mysql.createConnection(MYSQL_CONF).connect();` ;
      声明： `exec(sql){return new Promise(...)}` 并导出。
      - 连接 redis: `redisClient = redis.createClient(REDIS_CONF.port, REDIS_CONF.host)`。
      声明：`function set(key, val){redisClient.set(key, val, redis.print)}`并导出。
      声明：`function get(key) {return new Promise(...)}` 并导出。
      
