# vue-node-mysql
## This is a project based on amazeui+vue+node+express+bootstrap+mysql development.
## 该系统为建设一个高可扩展的H5企业网站，主要功能模块图如下：

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/mvc.png)

### 该系统分为前台用户子系统和后台企业管理子系统，具体系统用例图如下：

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/usecase01.png)

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/usecase.png)

### 根据功能性需求，使用starUML建立数据库E-R图如下：

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/dataer.png)

### 本系统需要安装通过install express cookie cookie-session body-parser  mysql express-static express-route multer consolidate ejs -D来安装模块依赖。

### 该系统的功能代码结构图如下：

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/jiegou.png)

### 该系统数据库表图如下：

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/shujuku.png)

#### 后台管理登录页 采用md5加密，主要功能代码为：<br>
const crypto=require('crypto');<br>
module.exports={
  MD5_SUFFIX: 'FDSW$t34tregt5tO&$(#RHuyoyiUYE*&OI$HRLuy87odlfh是个风格热腾腾)',
  md5: function (str){
    var obj=crypto.createHash('md5');<br>
    obj.update(str);<br> 
    return obj.digest('hex');
  } 
};<br>
之后再验证登录：
const express=require('express');<br>
const common=require('../../libs/common');<br>
const mysql=require('mysql');<br>
var db=mysql.createPool({host: 'localhost', user: 'root', password: 'wjl19840305', database: 'web'});<br>
module.exports=function (){<br>
  var router=express.Router();<br>
  router.get('/', (req, res)=>{
    res.render('admin/login.ejs', {});<br>
  });<br>
  router.post('/', (req, res)=>{
    var username=req.body.username;<br>
    var password=common.md5(req.body.password+common.MD5_SUFFIX);<br>
    db.query(`SELECT * FROM admin_table WHERE username='${username}'`, (err, data)=>{
      if(err){
        console.error(err);
        res.status(500).send('database error').end();<br>
      }else{
        if(data.length==0){
          res.status(400).send('no this admin').end();<br>
        }else{
          if(data[0].password==password){
            //成功
            req.session['admin_id']=data[0].ID;<br>
            res.redirect('/admin/');<br>
          }else{
            res.status(400).send('this password is incorrect').end();<br>
          }
        }
      }
    });<br>
  });<br>
  return router;<br>
};

后台登录页面效果图如下：

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/login01.png)

### 后台管理首页

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/02.png)

部分内容管理截图：<br>
![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/concent.png)

#### 公司案例产品管理页:<br>
![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/03.png)<br>

添加功能：![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/add.png)<br>

修改功能：![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/med.png)<br>

删除功能：![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/del.png)<br>

#### 合作伙伴管理

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/04.png)
![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/04.1.png)

#### 关于我们

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/05.png)

#### 联系我们

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/06.png)

### 前台页面

#### 前台注册登陆页与企业主页

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/login.png)
![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/index.png)

#### 新闻与新闻详情

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/news.png)
![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/news2.png)

#### 企业案例与产品

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/xinxi.png)
![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/xinxi1.png)

#### 企业服务

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/fuwu.png)

#### 企业文化与关于我们

![image](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/readme/aboutus.png)

### [详情请下载说明文档](https://github.com/k2-xu/vue-express-ejs-node-mysql/blob/master/%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3.doc "详细设计文档")

### 如有问题欢迎咨询本人QQ：326531916  微信：xu326531916 




#### mac  maridb 安装

Mac上安装mariadb
1、查看mariadb包信息

# brew info mariadb

mariadb: stable 10.2.6 (bottled)
Drop-in replacement for MySQL
https://mariadb.org/
Conflicts with: mariadb-connector-c, mysql, mysql-cluster, mysql-connector-c, mytop, percona-server
/usr/local/Cellar/mariadb/10.1.23 (594 files, 155.5MB) *
  Poured from bottle on 2017-05-14 at 17:49:24
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/mariadb.rb
==> Dependencies
Build: cmake ✔
Required: openssl ✔
==> Options
--with-archive-storage-engine
    Compile with the ARCHIVE storage engine enabled
--with-bench
    Keep benchmark app when installing
--with-blackhole-storage-engine
    Compile with the BLACKHOLE storage engine enabled
--with-embedded
    Build the embedded server
--with-libedit
    Compile with editline wrapper instead of readline
--with-local-infile
    Build with local infile loading support
--with-test
    Keep test when installing
==> Caveats
A "/etc/my.cnf" from another install may interfere with a Homebrew-built
server starting up correctly.

MySQL is configured to only allow connections from localhost by default

To connect:
    mysql -uroot

To have launchd start mariadb now and restart at login:
  brew services start mariadb
Or, if you don't want/need a background service you can just run:
  mysql.server start
2、安装mariadb

# brew install mariadb
3、运行Installer

# unset TMPDIR
# cd /usr/local/Cellar/mariadb/10.1.23/
# mysql_install_db
4、启动服务（包信息中有说明如何启动）

后台启动，并且开机自启动
# brew services start mariadb
==> Tapping homebrew/services
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 10 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (10/10), done.
Tapped 0 formulae (37 files, 51.1KB)
==> Successfully started `mariadb` (label: homebrew.mxcl.mariadb)

前台启动
# mysql.server start
Starting MySQL
.170529 21:36:17 mysqld_safe Logging to '/usr/local/var/mysql/bogon.err'.
170529 21:36:17 mysqld_safe Starting mysqld daemon with databases from /usr/local/var/mysql
 SUCCESS!
5、Secure the Installation

# mysql_secure_installation
6、查看版本

# mysql -uroot -p
MariaDB [(none)]> select @@version;
+-----------------+
| @@version       |
+-----------------+
| 10.1.23-MariaDB |
+-----------------+
1 row in set (0.00 sec)


### mysql 导入sql 数据

#### 创建数据库
mysql>  create database web;

#### 选择数据库
mysql>  use web;  

#### mysql>use database_name 
然后使用下面这个命令 

mysql>source web.sql   






