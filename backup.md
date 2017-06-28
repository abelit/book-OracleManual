### 贵州省信息中心数据库备份

### 1. 站群数据库

#### 1.1 站群服务器（前台，Web应用）

**IP:** 10.10.10.42

**User:** root

**Password: **gzxx@webfwq\#3012

#### **1.2 站群数据库（后台）**

* **服务器信息**

**Server: **

**System:**

**CPU:**

**Memory:**

**Disk:**

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

**Server: **

**System:**

**CPU:**

**Memory:**

**Disk:**

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

**Server: **

**System:**

**CPU:**

**Memory:**

**Disk:**

**IP:** 10.10.10.40

**User:** root

**Password: **

---

### **2. 省信息小型机**

#### 2.1 小机数据库\(公共信用平台\)

**服务器信息**

**Server: **IBM P6 520

**System:** AIX server02 1 6 00C71A624C00

**CPU: **

```
# pmcycles -m
CPU 0 runs at 4204 MHz
CPU 1 runs at 4204 MHz
CPU 2 runs at 4204 MHz
CPU 3 runs at 4204 MHz
CPU 4 runs at 4204 MHz
CPU 5 runs at 4204 MHz
CPU 6 runs at 4204 MHz
CPU 7 runs at 4204 MHz

# prtconf|grep Processors
Number Of Processors: 4
```

**Memory: **

```
# lsdev -Cc memory 
L2cache0 Available  L2 Cache
mem0     Available  Memory
# lsattr -El mem0 
ksh: There is not enough space in the file system.
ent_mem_cap          I/O memory entitlement in Kbytes           False
goodsize       63968 Amount of usable physical memory in Mbytes False
mem_exp_factor       Memory expansion factor                    False
size           63968 Total amount of physical memory in Mbytes  False
var_mem_weight       Variable memory capacity weight            False
# bootinfo -r 
ksh: There is not enough space in the file system.
65503232
```

**Disk: **

```
# df
ksh: There is not enough space in the file system.
Filesystem    512-blocks      Free %Used    Iused %Iused Mounted on
/dev/hd4        10485760         0  100%    53486    86% /
/dev/hd2        10485760   3745992   65%    53704    12% /usr
/dev/hd9var     10485760   9182216   13%     8310     1% /var
/dev/hd3        19398656   6011768   70%    14390     3% /tmp
/dev/hd1        10485760   6912264   35%     2019     1% /home
/proc                  -         -    -         -     -  /proc
/dev/hd10opt    10485760   9732408    8%    11393     2% /opt
/dev/fslv00    189792256  10875968   95%   228217    12% /oracle
```

**IP:** 59.215.244.67/68

**User:** root

**Password:** sxxzx.0851

**crontab: **

```
# crontab -l

0 1 * * * su - oracle -c "/home/oracle/exp_bak/MigrateByDatapump.sh backup allinone"

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

```
#!/bin/bash
#Function: backup and restore database with datapump by schemas
#Usage: crontab on linux/Unix  0 1 * * * su - oracle -c "/home/oracle/exp_bak/MigrateByDatapump.sh backup allinone"
#Author: Abelit
#Company: Guizhou Vision IT Co., Ltd.
#Created: 2017-06-26

############################################## Configuration Start ##########################################################################
#Set environment variable
export ORACLE_BASE=/oracle/11.4/oracle
export ORACLE_HOME=$ORACLE_BASE/11g

export ORACLE_BIN=$ORACLE_HOME/bin/
export PATH=$PATH:$ORACLE_BIN
export ORACLE_SID=orcl1
export NLS_LANG=AMERICAN_AMERICA.AL32UTF8

#Define variable and parameter
# If you want to import the objects from remote oracle database server, please create database link
# on the database and confifure database link name here e.a. 'DB_LINK'.
DB_LINK=
# DUMP_DIR_NAME is the directory of datapump that need.
DUMP_DIR_NAME=backup
DUMP_DIR=/oracle/expdp
FILE_SUFFIX=${ORACLE_SID}_`date +%Y%m%d%H%M`

# Set the time of expired backups
EXPIRED_TIME=1

# Set the numbers of parallel of datapump.
PARALLE_NUM=5
# List all schemas need to backup and configure 'ALL_SCHEMA' here.
ALL_SCHEMA="A8,A8OLD,AXS,DC,ETL,GZIC_SJJH,GZ_FGW,IM,IM_NEW,ORGINFO,SGS,SMARTBI,XYGS,XYGS_FY,XYGS_GAT,XY_DAJ_ZS,ZR_ISP"

############################################## Configuration End ##########################################################################

# BackupSchema funciton
function BackupSchema (){
    echo "BackupSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    expdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=${OWNER}_${FILE_SUFFIX}.dump logfile=${OWNER}_${FILE_SUFFIX}.log schemas=$OWNER $PARALLEL
    echo "BackupSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}

# BackupSchema funciton
function BackupAllSchema (){
    echo "BackupSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    expdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=AllSchemas_${FILE_SUFFIX}.dump logfile=AllSchemas_${FILE_SUFFIX}.log schemas=$OWNER $PARALLEL
    echo "BackupSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}

# BackupDatabase funciton
function BackupDatabase (){
    echo "BackupDatabase Start Time: `date` " >> $DUMP_DIR/datapump.log
    expdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=database_${FILE_SUFFIX}.dump logfile=database_${FILE_SUFFIX}.log full=y
    echo "BackupDatabase Finish Time: `date` " >> $DUMP_DIR/datapump.log
}

# RestoreSchema funciton
function RestoreSchema (){
    echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    impdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=${OWNER}_${FILE_SUFFIX}.dump logfile=${OWNER}_${FILE_SUFFIX}.log schemas=$OWNER remap_schema=$OWNER:$OWNER
    echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}
# RestoreSchema funciton
function RestoreAllSchema (){
    echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    impdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=AllSchemas_${FILE_SUFFIX}.dump logfile=AllSchemas_${FILE_SUFFIX}.log schemas=$OWNER
    echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}

# RestoreDatabase funciton
function RestoreDatabase (){
    echo "RestoreDatabase Start Time: `date` " >> $DUMP_DIR/datapump.log
    impdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME dumpfile=database_${FILE_SUFFIX}.dump logfile=database_${FILE_SUFFIX}.log full=y
    echo "RestoreDatabase Finish Time: `date` " >> $DUMP_DIR/datapump.log
}

# RestoreSchema funciton
function RestoreSchemaRemote (){
    echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    impdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME logfile=${OWNER}_${FILE_SUFFIX}.log network_link=$DB_LINK schemas=$OWNER remap_schema=$OWNER:$OWNER
    echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}

# RestoreSchema funciton
function RestoreAllSchemaRemote (){
    echo "RestoreSchema Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    impdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME logfile=AllSchemas_${FILE_SUFFIX}.log network_link=$DB_LINK schemas=$OWNER
    echo "RestoreSchema Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}

# RestoreDatabase funciton
function RestoreDatabaseRemote (){
    echo "RestoreDatabase Start Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
    impdp \'\/ as sysdba\' directory=$DUMP_DIR_NAME logfile=${OWNER}_${FILE_SUFFIX}.log network_link=$DB_LINK full=y
    echo "RestoreDatabase Finish Time: `date` OWNER: $OWNER" >> $DUMP_DIR/datapump.log
}

function DeleteExpiredFile () {
    # Delete expired files
    find $DUMP_DIR -name "*.dump" -mtime +$EXPIRED_TIME -exec rm {} \;
    find $DUMP_DIR -name "*.log" -mtime +$EXPIRED_TIME -exec rm {} \;
}

if [[ "$#" == 0 ]]; then
    echo "Invalid parameters, Please use '$0 help' for help!"
    exit 1
fi

if [[ "$1" == "help" ]]; then
    echo "Help: Usag of this script."
    echo "======================================="
    echo "Backup:  '$0 backup all/allinone/full/{assign_schema}' "
    echo "Restore: '$0 resore all/allinone/full/{assign_schema} {remote}' "
    exit 0
fi

if [[ "$PARALLE_NUM" != "" ]]; then
    PARALLEL="parallel="$PARALLE_NUM
fi

if [[ "$1" == "backup" ]]; then
    if [[ "$2" == "all" ]]; then
        ALL_SCHEMA = `echo $ALL_SCHEMA|sed 's/,/ /g'`
        for OWNER  in $ALL_SCHEMA
        do
            BackupSchema
        done
    elif [[ "$2" == "allinone" ]]; then
        OWNER=$ALL_SCHEMA
        BackupAllSchema
    elif [[ "$2" == "full" ]]; then
        BackupDatabase
    else
        OWNER=$2
        BackupSchema
    fi
elif [[ "$1" == "restore" ]]; then
    if [[ "$3" == "remote" ]]; then
        if [[ "$2" == "all" ]]; then
            ALL_SCHEMA = `echo $ALL_SCHEMA|sed 's/,/ /g'`
            for OWNER  in $ALL_SCHEMA
            do
                RestoreSchemaRemote
            done
        elif [[ "$2" == "allinone" ]]; then
            OWNER=$ALL_SCHEMA
            RestoreAllSchemaRemote
        elif [[ "$2" == "full" ]]; then
            RestoreDatabaseRemote
        else
            OWNER=$2
            RestoreSchemaRemote
        fi
    else
        if [[ "$2" == "all" ]]; then
            ALL_SCHEMA = `echo $ALL_SCHEMA|sed 's/,/ /g'`
            for OWNER  in $ALL_SCHEMA
            do
                RestoreSchema
            done
        elif [[ "$2" == "allinone" ]]; then
            OWNER=$ALL_SCHEMA
            RestoreAllSchema
        elif [[ "$2" == "full" ]]; then
            RestoreDatabase
        else
            OWNER=$2
            RestoreSchema
        fi
    fi
else
    echo "Invalid parameters, Please use '$0 help' for help!"
    exit 1
fi

if [[ "$EXPIRED_TIME" = "" ]]; then
    exit 0
else
    DeleteExpiredFile
fi

exit 0
```
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

