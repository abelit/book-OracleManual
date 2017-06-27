### 贵州省信息中心数据库备份

### 1. 站群数据库

#### 1.1 站群服务器（前台）

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

```

```

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

**数据库信息**

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

```

```

#### 1.4 站群数据库（备份）

**IP:** 10.10.10.40

**User:** root

**Password: **

---

### **2. 省信息小型机**

#### 2.1 小机数据库

**IP:** 59.215.244.67/68

**User:** root

**Password:** sxxzx.0851

**backup\_method: **datapump

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

