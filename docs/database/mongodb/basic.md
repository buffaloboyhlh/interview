# MongoDB 基础

MongoDB 是一个面向文档的 NoSQL 数据库系统，具有高性能、高可用性和易扩展性。下面是 MongoDB 的详细使用教程，涵盖从安装、基本操作、索引到高级功能如聚合和复制集的内容。

### 一、MongoDB 简介

MongoDB 使用 BSON（Binary JSON）格式存储数据，数据存储在类似于 JSON 的文档中。这使得它特别适合处理非结构化或半结构化数据，同时提供了灵活的数据模式。

### 二、MongoDB 安装

#### 2.1 在 Ubuntu 上安装 MongoDB
1. **更新系统包列表并安装 MongoDB：**
   ```bash
   sudo apt-get update
   sudo apt-get install -y mongodb
   ```

2. **启动 MongoDB 服务：**
   ```bash
   sudo systemctl start mongodb
   sudo systemctl enable mongodb
   ```

3. **检查 MongoDB 是否成功运行：**
   ```bash
   sudo systemctl status mongodb
   ```

#### 2.2 在 Windows 上安装 MongoDB
1. **下载 MongoDB MSI 安装包：**
   - 前往 [MongoDB 下载页面](https://www.mongodb.com/try/download/community) 并选择适合的版本。

2. **安装 MongoDB：**
   - 双击下载的 MSI 文件，按照提示完成安装。

3. **启动 MongoDB 服务：**
   - 打开命令提示符，输入以下命令启动服务：
     ```cmd
     net start MongoDB
     ```

#### 2.3 在 macOS 上安装 MongoDB
1. **使用 Homebrew 安装 MongoDB：**
   ```bash
   brew tap mongodb/brew
   brew install mongodb-community
   ```

2. **启动 MongoDB 服务：**
   ```bash
   brew services start mongodb/brew/mongodb-community
   ```

### 三、MongoDB 基本操作

#### 3.1 连接 MongoDB
MongoDB 默认在本地 `localhost` 的 `27017` 端口运行。可以使用 MongoDB shell 或其他客户端工具连接到数据库。

```bash
mongo
```

#### 3.2 数据库操作
- **显示所有数据库：**
  ```js
  show dbs;
  ```

- **创建或切换数据库：**
  ```js
  use mydatabase;
  ```

- **删除数据库：**
  ```js
  db.dropDatabase();
  ```

#### 3.3 集合操作
- **创建集合：**
  ```js
  db.createCollection("mycollection");
  ```

- **查看集合：**
  ```js
  show collections;
  ```

- **删除集合：**
  ```js
  db.mycollection.drop();
  ```

#### 3.4 文档操作

##### 插入文档
- **插入一个文档：**
  ```js
  db.mycollection.insertOne({name: "Alice", age: 25});
  ```

- **插入多个文档：**
  ```js
  db.mycollection.insertMany([
      {name: "Bob", age: 30},
      {name: "Charlie", age: 35}
  ]);
  ```

##### 查询文档
- **查询所有文档：**
  ```js
  db.mycollection.find();
  ```

- **根据条件查询文档：**
  ```js
  db.mycollection.find({name: "Alice"});
  ```

- **查询单个文档：**
  ```js
  db.mycollection.findOne({name: "Alice"});
  ```

##### 更新文档
- **更新一个文档：**
  ```js
  db.mycollection.updateOne(
      {name: "Alice"}, 
      {$set: {age: 26}}
  );
  ```

- **更新多个文档：**
  ```js
  db.mycollection.updateMany(
      {age: {$gt: 25}}, 
      {$set: {status: "active"}}
  );
  ```

##### 删除文档
- **删除一个文档：**
  ```js
  db.mycollection.deleteOne({name: "Alice"});
  ```

- **删除多个文档：**
  ```js
  db.mycollection.deleteMany({age: {$lt: 30}});
  ```

### 四、MongoDB 索引

MongoDB 通过索引提高查询性能。

#### 4.1 创建索引
- **单字段索引：**
  ```js
  db.mycollection.createIndex({name: 1});
  ```

- **复合索引：**
  ```js
  db.mycollection.createIndex({name: 1, age: -1});
  ```

#### 4.2 查看索引
```js
db.mycollection.getIndexes();
```

#### 4.3 删除索引
- **删除特定索引：**
  ```js
  db.mycollection.dropIndex("name_1");
  ```

- **删除所有索引：**
  ```js
  db.mycollection.dropIndexes();
  ```

### 五、MongoDB 聚合

MongoDB 的聚合功能类似于 SQL 中的 `GROUP BY`，可以处理和汇总数据。

#### 5.1 基本聚合操作
```js
db.mycollection.aggregate([
    {$match: {status: "active"}},
    {$group: {_id: "$age", total: {$sum: 1}}},
    {$sort: {total: -1}}
]);
```

#### 5.2 聚合管道
聚合管道包含多个阶段，每个阶段执行特定的操作。
```js
db.mycollection.aggregate([
    {$match: {status: "active"}},
    {$group: {_id: "$age", total: {$sum: 1}}},
    {$project: {age: "$_id", total: 1, _id: 0}},
    {$sort: {total: -1}},
    {$limit: 10}
]);
```

### 六、MongoDB 复制集

复制集是 MongoDB 的高可用性解决方案，通过多个节点保持数据副本。

#### 6.1 设置复制集
1. **在 `mongod` 配置文件中配置复制集名称：**
   ```yaml
   replication:
     replSetName: "rs0"
   ```

2. **启动 `mongod` 实例并初始化复制集：**
   ```js
   rs.initiate();
   rs.add("hostname:port");
   ```

#### 6.2 复制集常用命令
- **查看复制集状态：**
  ```js
  rs.status();
  ```

- **添加成员：**
  ```js
  rs.add("new_member_hostname:port");
  ```

- **移除成员：**
  ```js
  rs.remove("member_hostname:port");
  ```

### 七、MongoDB 分片

分片用于在多台服务器上分布存储数据，以支持大规模数据集和高吞吐量。

#### 7.1 启用分片
1. **启动配置服务器和分片服务器：**

2. **启动 `mongos` 并配置分片：**
   ```js
   sh.enableSharding("mydatabase");
   sh.shardCollection("mydatabase.mycollection", {shardKey: 1});
   ```

### 八、MongoDB 安全性

#### 8.1 用户管理
- **创建管理员用户：**
  ```js
  use admin;
  db.createUser({
      user: "admin",
      pwd: "password",
      roles: [{role: "root", db: "admin"}]
  });
  ```

- **创建普通用户：**
  ```js
  use mydatabase;
  db.createUser({
      user: "myuser",
      pwd: "mypassword",
      roles: [{role: "readWrite", db: "mydatabase"}]
  });
  ```

#### 8.2 角色管理
- **授予角色：**
  ```js
  db.grantRolesToUser("myuser", [{role: "dbAdmin", db: "mydatabase"}]);
  ```

- **撤销角色：**
  ```js
  db.revokeRolesFromUser("myuser", [{role: "readWrite", db: "mydatabase"}]);
  ```

### 九、MongoDB 备份与恢复

#### 9.1 备份数据
使用 `mongodump` 工具备份数据库：
```bash
mongodump --out /path/to/backup --db mydatabase
```

#### 9.2 恢复数据
使用 `mongorestore` 工具恢复数据：
```bash
mongorestore /path/to/backup --db mydatabase
```

### 十、MongoDB 性能优化

#### 10.1 使用索引优化查询性能
- **分析查询：**
  ```js
  db.mycollection.find({name: "Alice"}).explain("executionStats");
  ```

#### 10.2 存储引擎选择
根据应用场景选择合适的存储引擎，如 `WiredTiger`、`In-Memory` 等。

### 结语

MongoDB 是一个功能强大且灵活的数据库系统，适合各种大数据处理场景。通过本教程的学习，你可以掌握 MongoDB 的基本使用技巧，并能根据具体需求进行高级配置和优化。