Session: 1

SQL> select * from dept;
DEPTNO     DNAME          LOC
---------- -------------- -------------
10         ACCOUNTING     NEW YORK
20         RESEARCH       DALLAS
30         SALES          CHICAGO
40         OPERATIONS     BOSTON

Lock one row.

SQL> update dept set dname='DEV' where deptno=30;
1 row updated.

Do not commit

Session: 2

Try to lock the same row

SQL> update dept set dname='TESTING' where deptno=30;

Execute this srcipts to gather lock statistic.

SQL> select (select username from v$session where sid=a.sid) BLOCKER,
2 a.sid,
3 'IS BLOCKING',
4 (select username from v$session where sid=b.sid) BLOCKEE,
5 b.sid
6 from v$lock a, v$lock b
7 where a.block = 1
8 and b.request > 0
9 and a.id1 = b.id1
10 and b.id2 = b.id2
11 /


BLOCKER                        SID        'ISBLOCKING BLOCKEE                        SID
------------------------------ ---------- ----------- ------------------------------ ----------
SCOTT                          146        IS BLOCKING SCOTT                          151


SQL> col OBJ format a15
SQL> col SS format a15
SQL> set linesize 200


SQL> select owner||'.'||object_name obj
2 ,oracle_username||' ('||s.status||')' oruser
3 ,os_user_name osuser
4 ,l.process unix
5 ,''''||s.sid||','||s.serial#||'''' ss
6 ,r.name rs
7 ,to_char(s.logon_time,'yyyy/mm/dd hh24:mi:ss') time
8 from v$locked_object l
9 ,dba_objects o
10 ,v$session s
11 ,v$transaction t
12 ,v$rollname r
13 where l.object_id = o.object_id
14 and s.sid=l.session_id
15 and s.taddr=t.addr
16 and t.xidusn=r.usn
17 order by osuser, ss, obj
18 /


OBJ             ORUSER            OSUSER                         UNIX         SS RS TIME
--------------- ----------------- ------------------------------ ------------ --------------- ------------------------------ -------------------
SCOTT.DEPT      SCOTT (INACTIVE)  oracle                          17809       '146,188'

_SYSSMU1$ 2009/08/04 06:03:37

here ss means <session_id>,<serial#>

Session: 3

Kill the session to release the lock:

ALTER SYSTEM KILL SESSION ÎéÎ÷<session_id>,<serial#>ÎéÎ÷;


SQL> alter system kill session '146,188';

System altered.
