# time 库

* time()    返回当前时间（单位秒）
* ctime()    返回一个表示当前时间的字符串
* gmtime()    返回一个time结构体，用于strftime中的tf
* perf_counter()    CPU精确时间计数值（单位秒）
* sleep(s)    让程序暂停（单位秒）

* strftime(tpl,tf)   tpl为格式化的字符串（参考下面），tf为time结构体（可以使用gmtime()获得）
* %Y    年份
* %m    月份数字
* %B    月份名称
* %b    月份名称缩写
* %d    日期
* %A    星期
* %a    星期缩写
* %H    24小时制时间
* %h    12小时制时间
* %p    AM/PM
* %M    分钟
* %S    秒
