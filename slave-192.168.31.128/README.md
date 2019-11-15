# slave设置
从库的mysql安装跟master一样。
基于docker进行安装

只要能连接上即可

## slave my.cnf配置

![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/slave-192.168.31.128/1573800577.jpg)

局域网中，server-id 是作为唯一标识(若一台机器多个mysql，则server-id也不同)


### 查看从库同步状态
进入slave-mysql中
执行  `show slave status \G;`

![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/slave-192.168.31.128/1573801845.png)

SlaveIORunning 和 SlaveSQLRunning 都是No

未开启主从复制

## 开启从库复制
### 进入master获取File和position值
进入master的mysql命令行，执行

`show master status;`

![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/slave-192.168.31.128/1573800977.jpg)

获取到这两个值后，不要执行任何命令，不然会改变position值

切换到slave的mysql

`change master to master_host='192.168.31.129', master_user='slave', master_password='000000',
 master_port=3306,
 master_log_file='mysql-bin.000001', master_log_pos= 1480, master_connect_retry=30;`
 
 **master_host** 为mysql的宿主机IP
 
 **master_user** 在master中添加的user账号
 
 **master_password** 在master中添加的user密码
 
 **master_port** master-mysql的端口
 
 **master_log_file** 刚在master获取的file值
 
 **master_log_pos** 刚在master获取的position值
 
 **master_connect_retry** 超时重连时间(s) 默认为60s
 
 
 
 ### 查看从库同步状态
 进入slave-mysql中，执行
 
 `show slave status \G;`
 
 **\G**：格式化
 
 ![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/slave-192.168.31.128/1573801645.png)
 
 会发现刚刚的 **SlaveIORunning** 和 **SlaveSQLRunning** 都变成 **Yes**
 
 
 ## 测试
 
 在129的主库中，添加一个名为test的数据库，创建一个名为test的表，插入一条记录
 
 ![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/slave-192.168.31.128/1573802080.jpg)
 
 然后，打开128的从库，查看 
 
 ![docker images](https://github.com/yonghengee/mysql-master-slave/blob/master/slave-192.168.31.128/1573802234.jpg)
 
 数据复制过来了!
 
 简单的主从复制完成。
 
 
 









