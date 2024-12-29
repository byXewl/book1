
## **sqlite和mysql的不同**

熟悉MySQL数据库的人都知道，MySQL中有一个名为information_schema的系统库，里面包含了所有MYSQL数据库中库名，表名，列名的信息，那么SQLITE数据库有没有呢？
答案当然是没有的。对于SQLITE而言，并没有库的概念，而是直接对象就是表了，所以SQLITE没有系统库，但是它是存在系统表的，这个表名为sqlite_master

