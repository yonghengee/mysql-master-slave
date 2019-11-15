# master设置
### 使用docker安装mysql
`docker images` 查看当前docker安装的镜像

![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573798160.png)

`docker pull mysql:5.7`这里使用5.7的mysql，其他版本请自行安装

`docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql `
其中 **mysql-test** 为容器名称
 **3306:3306**冒号前的端口为容器端口，后着为宿主机端口

`docker ps`
查看当前运行的容器
![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573799003.jpg)


`docker exec -it mysql-test /bin/bash`
进入容器内
![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573799134.jpg)

现在可以使用`mysql -u root -p`
输入密码进入mysql
![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573799221.jpg)


### 使用navicat查看是否可以连接
![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573799295.jpg)

### 安装完毕

### 配置Master
`apt-get update` && `apt-get install vim`
先进入容器中装载vim进行文本编辑

`cd etc/mysql/`
进入mysql配置目录，查看my.cnf
 `vim my.cnf`
![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573799709.jpg)

在引用的任意文件中(或者新加一个文件)
加入以下参数

![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/master-192.168.31.129/1573799984.jpg)

退出。
重启mysql `service mysql restart` 
这时会退出docker容器，并且需要重新启动mysql
`docker start mysql-test`

### 配置slave用户
进入docker容器的mysql中，执行
`CREATE USER 'slave'@'%' IDENTIFIED BY '000000';`

`GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';`
授予用户 slave ,REPLICATION SLAVE权限和REPLICATION CLIENT权限，用于在从库同步主库数据。







