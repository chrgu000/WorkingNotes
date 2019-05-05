## 如何终结掉卡死的进程？

在ORACLE数据库杀掉会话进程有三种方式：

**一、ALTER SYSTEM KILL SESSION**

​	关于KILL SESSION Clause ，如下官方文档描述所示，alter system kill session实际上不是真正的杀死会话，它只是将会话标记为终止。等待PMON进程来清除会话。

​	可以使用ALTER SYSTEM KILL SESSION 'sid,serial#' IMMEDIATE 来快速回滚事物、释放会话的相关锁、立即返回当前会话的控制权。 

**二、ALTER SYSTEM DISCONNECT SESSION**

​	ALTER SYSTEM DISCONNECT SESSION 杀掉专用服务器(DEDICATED SERVER)或共享服务器的连接会话，它等价于从操作系统杀掉进程。它有两个选项POST_TRANSACTION和IMMEDIATE， 其中POST_TRANSACTION表示等待事务完成后断开会话，IMMEDIATE表示中断会话，立即回滚事务。

```mysql
SQL> ALTER SYSTEM DISCONNECT SESSION 'sid,serial#' POST_TRANSACTION;  
SQL> ALTER SYSTEM DISCONNECT SESSION 'sid,serial#' IMMEDIATE; 
```

**三、KILL -9 SPID （Linux） 或 orakill ORACLE_SID spid （Windows）**

​	可以使用下面SQL语句找到对应的操作系统进程SPID，然后杀掉。当然杀掉操作系统进程是一件危险的事情，尤其不要误杀。所以在执行前，一定要谨慎确认。

```mysql
SET LINESIZE 100
COLUMN spid FORMAT A10
COLUMN username FORMAT A10
COLUMN program FORMAT A45
 
SELECT s.inst_id,
       s.sid,
       s.serial#,
       p.spid,
       s.username,
       s.program
FROM   gv$session s
       JOIN gv$process p ON p.addr = s.paddr AND p.inst_id = s.inst_id
WHERE  s.type != 'BACKGROUND';
```



在数据库如果要彻底杀掉一个会话，尤其是大事务会话，==最好是使用ALTER SYSTEM DISCONNECT SESSION IMMEDIATE==或使用下面步骤： 

1：首先在操作系统级别Kill掉进程。 

2：在数据库内部KILL SESSION 

或者反过来亦可。这样可以快速终止进程，释放资源。 



## sql语句写法—As后的别名中有括号

```mysql
select 0 as "20条法则（元）" from dual;
```

