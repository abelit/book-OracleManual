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

    #!/bin/bash
    #Function: backup and restore database with datapump by schemas
    #Usage: crontab on linux/Unix
    #Author: Abelit
    #Company: Guizhou Vision IT Co., Ltd.
    #Created: 2017-06-26

    #Set environment variable
    export ORACLE_BASE=/data/oracle
    export ORACLE_HOME=/data/oracle/product/10.2
    export ORACLE_SID=gzxx
    export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
    export ORACLE_BIN=$ORACLE_HOME/bin/
    export PATH=$PATH:ORACLE_BIN

    #Define variable and parameter
    # DUMP_DIR_NAME is the directory of datapump that need.
    DBLINK=
    DUMP_DIR_NAME=DUMP
    DUMP_DIR=/data/dump_dir
    FILE_SUFFIX=`date +%Y%m%d%H%M`

    # Variable for this shell
    PARA=$2

    # List all schemas need to backup and configure 'ALL_USERS' here.
    ALL_USERS="JCMS25GZ JCMSGZXX JGETGZXX JIEPGZ JISGZXX JSEARCHGZXX LC LM TYSP TYSP_SYSTEM VCGZXX WEBSITE WYCHEN"

    ALLINONE="JCMS25GZ,JCMSGZXX,JGETGZXX,JIEPGZ,JISGZXX,JSEARCHGZXX,LC,LM,TYSP,TYSP_SYSTEM,VCGZXX,WEBSITE,WYCHEN"

    # BackupSchema funciton
    function BackupSchema (){
        echo "BackupSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
        expdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=${OWNER}_${FILE_SUFFIX}.dump logfile=${OWNER}_${FILE_SU
    FFIX}.log schemas=$OWNER
        echo "BackupSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    }

    # BackupSchema funciton
    function BackupAllSchema (){
        echo "BackupSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
        expdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=AllSchemas_${FILE_SUFFIX}.dump logfile=AllSchemas_${FILE_SU
    FFIX}.log schemas=$OWNER
        echo "BackupSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    }

    # BackupDatabase funciton
    function BackupDatabase (){
        echo "BackupDatabase Start Time: `date` " >> $DUMP_DIR/datapump.log
        expdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=database_${FILE_SUFFIX}.dump logfile=database_${FILE_SU
    FFIX}.log full=y
        echo "BackupDatabase Finish Time: `date` " >> $DUMP_DIR/datapump.log
    }

    # RestoreSchema funciton
    function RestoreSchema (){
        echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
        impdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=${OWNER}_${FILE_SUFFIX}.dump logfile=${OWNER}_${FILE_SU
    FFIX}.log schemas=$OWNER remap_schema=$OWNER:$OWNER
        echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    }
    # RestoreSchema funciton
    function RestoreAllSchema (){
        echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
        impdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=AllSchemas_${FILE_SUFFIX}.dump logfile=AllSchemas_${FIL
    E_SUFFIX}.log schemas=$OWNER
        echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    }

    # RestoreDatabase funciton
    function RestoreDatabase (){
        echo "RestoreDatabase Start Time: `date` " >> $DUMP_DIR/datapump.log
        impdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=database_${FILE_SUFFIX}.dump logfile=database_${FILE_SU
    FFIX}.log full=y
        echo "RestoreDatabase Finish Time: `date` " >> $DUMP_DIR/datapump.log
    }

    # RestoreSchema funciton
    function RestoreSchemaRemote (){
        echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
        impdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=${OWNER}_${FILE_SUFFIX}.dump logfile=${OWNER}_${FILE_SU
    FFIX}.log network_link=$DBLINK schemas=$OWNER remap_schema=$OWNER:$OWNER
        echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    }

    # RestoreDatabase funciton
    function RestoreDatabaseRemote (){
        echo "RestoreDatabase Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
        impdp \'sys \/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=${OWNER}_${FILE_SUFFIX}.dump logfile=${OWNER}_${FILE_SU
    FFIX}.log network_link=$DBLINK full=y
        echo "RestoreDatabase Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    }

    if [ "$1" = "backup" ]; then
        if [ "$PARA" = "all" ]; then
            for OWNER  in $ALL_USERS
            do
                BackupSchema
            done
        elif [ "$PARA" = "allinone" ]; then
            OWNER=$ALLINONE
            BackupAllSchema
        else
            OWNER=$PARA
            BackupSchema
        fi
    elif [ "$1" = "restore" ]; then
        if [ "$PARA" = "all" ]; then
            for OWNER  in $ALL_USERS
            do
                RestoreSchema
            done
        elif [ "$PARA" = "allinone" ]; then
            OWNER=$ALLINONE
            RestoreAllSchema
        else
            OWNER=$PARA
            RestoreSchema
        fi
    else
        echo "Invalid parameters!"
    fi

    # Delete expired files
    find $DUMP_DIR -name "*.dump" -mtime +15 -exec rm {} \;
    find $DUMP_DIR -name "*.log" -mtime +15 -exec rm {} \;


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

