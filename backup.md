### 贵州省信息中心数据库备份

### 1. 站群数据库

#### 1.1 站群服务器（前台，Web应用）

**IP:** 10.10.10.42

**User:** root

**Password: **gzxx@webfwq\#3012

#### **1.2 站群数据库（后台）**

* **服务器信息**

**IP:** 10.10.10.41

**User:** root

**Password:** gzxx@323\#

**crontab: **

```
[root@localhost ~]# crontab -l
0 1 * * * su - oracle -c "/data/oracledba/MigrateByDatapump.sh backup allinone"
```

* **数据库信息**

**oracle\_version: **10.2.0.4.0

**oracle\_character: **

```
SQL> select userenv('LANGUAGE') from dual;

USERENV('LANGUAGE')
----------------------------------------------------
AMERICAN_AMERICA.AL32UTF8
```

**backup\_method: **datapump by schema

**backup\_schemas: **

```
SQL> select username from dba_users where account_status='OPEN' order by username;

JCMS25GZ
JCMSGZXX
JGETGZXX
JIEPGZ
JISGZXX
JSEARCHGZXX
LC
LM
TYSP
TYSP_SYSTEM
VCGZXX
WEBSITE
WYCHEN
```

**backup\_directory: **

```
SQL> select * from dba_directories;

OWNER DIRECTORY_NAME       DIRECTORY_PATH
----- -------------------- --------------------------------------------------
SYS   DUMP           /data/dump_dir
```

**backup\_script: **

#### 1.3 站群数据库

* **服务器信息**

**IP:** 10.10.10.44

**User:** root

**Password:** oracle2014

**crontab:**

```
[root@oracle ~]# crontab -l
0 1 * * * su - oracle -c "/u01/app/oracle/exp_bak/exp_backup.sh > /u01/app/oracle/exp_bak/backup_exp.log 2>&1"
```

* **数据库信息**

**oracle\_version: **11.2.0.3.0

**oracle\_character: **

```
SQL> select userenv('LANGUAGE') from dual;

USERENV('LANGUAGE')
----------------------------------------------------
AMERICAN_AMERICA.AL32UTF8
```

**backup\_method: **datapump by schema

**backup\_schemas: **

```
SQL> select username from dba_users where account_status='OPEN' order by username;
JCMS1
JCMS2
JCMS25GZ
JCMSDOWN
JCMSGZXX
JGETGZXX
JIEPGZ
JISGZXX
JSEARCHGZXX
LC
LM
TYSP
TYSP_SYSTEM
VCGZXX
WEBSITE
```

**backup\_directory: **

```
SQL> select * from dba_directories;

OWNER DIRECTORY_NAME       DIRECTORY_PATH
----- -------------------- --------------------------------------------------
SYS   DUMP           /data/dump_dir
```

**backup\_script: **

```

```

#### 1.4 站群数据库（备份）

**IP:** 10.10.10.40

**User:** root

**Password: **

---

### **2. 省信息小型机**

#### 2.1 小机数据库\(公共信用平台\)

**服务器信息**

**Server: **IBM P6 520

**System:** AIX server02 1 6 00C71A624C00

**IP:** 59.215.244.67/68

**User:** root

**Password:** sxxzx.0851

**crontab: **

```
# crontab -l
# @(#)08        1.15.1.3  src/bos/usr/sbin/cron/root, cmdcntl, bos610 2/11/94 17:19:47
# IBM_PROLOG_BEGIN_TAG 
# This is an automatically generated prolog. 
#  
# bos610 src/bos/usr/sbin/cron/root 1.15.1.3 
#  
# Licensed Materials - Property of IBM 
#  
# COPYRIGHT International Business Machines Corp. 1989,1994 
# All Rights Reserved 
#  
# US Government Users Restricted Rights - Use, duplication or 
# disclosure restricted by GSA ADP Schedule Contract with IBM Corp. 
#  
# IBM_PROLOG_END_TAG 
#
# COMPONENT_NAME: (CMDCNTL) commands needed for basic system needs
#
# FUNCTIONS: 
#
# ORIGINS: 27
#
# (C) COPYRIGHT International Business Machines Corp. 1989,1994
# All Rights Reserved
# Licensed Materials - Property of IBM
#
# US Government Users Restricted Rights - Use, duplication or
# disclosure restricted by GSA ADP Schedule Contract with IBM Corp.
#
#0 3 * * * /usr/sbin/skulker
#45 2 * * 0 /usr/lib/spell/compress
#45 23 * * * ulimit 5000; /usr/lib/smdemon.cleanu > /dev/null
0 11 * * * /usr/bin/errclear -d S,O 30
0 12 * * * /usr/bin/errclear -d H 90
0,5,10,15,20,25,30,35,40,45,50,55 * * * * /usr/sbin/dumpctrl -k >/dev/null 2>/dev/null
0 15 * * *  /usr/lib/ras/dumpcheck >/dev/null 2>&1
55 23 * * * /var/perf/pm/bin/pmcfg  >/dev/null 2>&1     #Enable PM Data Collection
0 1 * * * su - oracle -c "/home/oracle/exp_bak/backup_exp.sh >> /home/oracle/exp_bak/backup_exp.log 2>&1"
```

* **数据库信息**

**oracle\_version: **11.2.0.4.0

**oracle\_character: **

```
SQL> select userenv('LANGUAGE') from dual;

USERENV('LANGUAGE')
----------------------------------------------------
AMERICAN_AMERICA.AL32UTF8
```

**backup\_method: **datapump by schema

**backup\_schemas: **

```
SQL> select username from dba_users where account_status='OPEN' order by username;

JCMS25GZ
JCMSGZXX
JGETGZXX
JIEPGZ
JISGZXX
JSEARCHGZXX
LC
LM
TYSP
TYSP_SYSTEM
VCGZXX
WEBSITE
WYCHEN
```

**backup\_directory: **

```
SQL> select * from dba_directories;

OWNER DIRECTORY_NAME       DIRECTORY_PATH
----- -------------------- --------------------------------------------------
SYS   DUMP           /data/dump_dir
```

**backup\_script: **

#### 2.2 小机数据库（备份）

**IP:** 59.215.241.27

**User:** root

**Password:** xxzx@123

---

### 3.协同办公

### 3.1 协同办公数据库

**IP:** 59.215.233.125

**User:** root

**Password:** visionit

**backup\_method: **datapump

**backup\_script: **

#### 3.2 协同办公数据库（备份）

**IP:** 59.215.241.27

**User:** root

**Password:** xxzx@123

