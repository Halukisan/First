1. 管理员调用命令行
window10下在Windows/system32里找cmd
1. mysqld --skip-grant-tables



1. 重新再以管理员调用命令行

2. mysql -u root

3. use mysql;

4. show tables;

5. update mysql.user set authentication_string=password('123456')where user='123456';

   > update user set password=password(‘新密码’) where user=‘root’ and host=‘localhost’;

6. flush privileges;

7. quit



1. 打开mysql，密码已经更新。