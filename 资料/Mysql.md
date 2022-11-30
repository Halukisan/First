```
set @user1 = 'lee',@user2 = 'sun';
SELECT @user1,@user2;
```
```
SELECT CURRENT_DATE;
```
```
SELECT Length('hello world');
```
```
SELECT Current_time();
```

### Mysql 函数
* 数学函数
1. Rand()用于返回0-1随机浮点数。如select rand();
2. Sqrt()用于返回参数的平方根。如select sqrt(4);
3. ABS()用于返回参数的绝对值。如select abs(3),abs(-3);
4. Floor()函数用于返回小于或等于参数的最大整数值（小）
5. Ceiling()用于返回大于或者等于参数的最小整数值（大）。
6. Round()用于返回四舍五入后的整数

* 字符串函数
1. Length()用于返回参数的长度，返回值为整数。
2. Left(s,n)和Right(s,n)函数分别返回字符串s左侧和右侧开始的n个字符。
3. Curdate()和Current_date()函数用于返回当前日期。
4. Cutime()和Current_time()函数用于返回当前时间。
5. Now()用于返回当前日期和时间。
6. Version()用于返回数据库版本号。



