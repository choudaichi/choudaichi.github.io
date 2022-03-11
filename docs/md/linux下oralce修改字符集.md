# 修改docker中oracle的默认字符集
```bash
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export PATH=$ORACLE_HOME/bin:$PATH
export ORACLE_SID=helowin
```
首先导出这些，然后进入sqlplus，一定要记得加上SID否则有问题
```sql
[oracle@localhost ~]$ sqlplus / as sysdba

SQL*Plus: Release 11.2.0.1.0 Production on Thu Jun 25 10:41:57 2020

Copyright (c) 1982, 2009, Oracle.  All rights reserved.


Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
SQL> select * from v$nls_parameters where parameter='NLS_CHARACTERSET';

PARAMETER
--------------------------------------------------------------------------------
VALUE
--------------------------------------------------------------------------------
NLS_CHARACTERSET
AL32UTF8


SQL> shutdown immediate;
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> startup mount
ORACLE instance started.

Total System Global Area 1.3362E+10 bytes
Fixed Size                  2217952 bytes
Variable Size            6777997344 bytes
Database Buffers         6576668672 bytes
Redo Buffers                4960256 bytes
Database mounted.
SQL> ALTER SYSTEM ENABLE RESTRICTED SESSION;

System altered.

SQL> ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;

System altered.

SQL> ALTER SYSTEM SET AQ_TM_PROCESSES=0;

System altered.

SQL>  alter database open;

Database altered.

SQL> ALTER DATABASE CHARACTER SET ZHS16GBK;
ALTER DATABASE CHARACTER SET ZHS16GBK
*
ERROR at line 1:
ORA-12712: new character set must be a superset of old character set


SQL> ALTER DATABASE character set INTERNAL_USE ZHS16GBK;

Database altered.

SQL> select * from v$nls_parameters;

PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_LANGUAGE
AMERICAN

NLS_TERRITORY
AMERICA

NLS_CURRENCY
$


PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_ISO_CURRENCY
AMERICA

NLS_NUMERIC_CHARACTERS
.,

NLS_CALENDAR
GREGORIAN


PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_DATE_FORMAT
DD-MON-RR

NLS_DATE_LANGUAGE
AMERICAN

NLS_CHARACTERSET
ZHS16GBK


PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_SORT
BINARY

NLS_TIME_FORMAT
HH.MI.SSXFF AM

NLS_TIMESTAMP_FORMAT
DD-MON-RR HH.MI.SSXFF AM


PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_TIME_TZ_FORMAT
HH.MI.SSXFF AM TZR

NLS_TIMESTAMP_TZ_FORMAT
DD-MON-RR HH.MI.SSXFF AM TZR

NLS_DUAL_CURRENCY
$


PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_NCHAR_CHARACTERSET
AL16UTF16

NLS_COMP
BINARY

NLS_LENGTH_SEMANTICS
BYTE


PARAMETER
----------------------------------------------------------------
VALUE
----------------------------------------------------------------
NLS_NCHAR_CONV_EXCP
FALSE


19 rows selected.

SQL> shutdown immediate;
Database closed.
Database dismounted.
ORACLE instance shut down.
SQL> startup
ORACLE instance started.

Total System Global Area 1.3362E+10 bytes
Fixed Size                  2217952 bytes
Variable Size            6777997344 bytes
Database Buffers         6576668672 bytes
Redo Buffers                4960256 bytes
Database mounted.
Database opened.
SQL>
SQL> select * from nls_database_parameters t where t.parameter='NLS_CHARACTERSET';

PARAMETER
------------------------------
VALUE
--------------------------------------------------------------------------------
NLS_CHARACTERSET
ZHS16GBK

```

