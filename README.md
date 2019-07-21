# node-blog
**node-blog-原生实现**

## 项目从服务到数据经过四层拆分
1. www.js 层：  创建服务，监听端口
2. app.js 层：  解析 path query 引入路由 处理404
3. router 层：  处理路由
4. controller层：  处理数据
