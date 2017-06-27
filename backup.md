### 贵州省信息中心数据库备份

### 1. 站群数据库

#### 1.1 站群数据库（前台）

**IP:** 10.10.10.42

**User:** root

**Password: **gzxx@webfwq\#3012

**backup\_method: **datapump

**backup\_script: **

#### **1.2 站群数据库（后台）**

**IP:** 10.10.10.41

**User:** root

**Password:** gzxx@323\#

**backup\_method: **datapump

**backup\_script: **

    #!/bin/sh
    #Function: backup database with expdp by schemas
    #Usage: crontab on linux/Unix
    #Author: Abelit
    #Created: 2017-05-25

    #Set environment variable
    export ORACLE_BASE=/data/oracle
    export ORACLE_HOME=/data/oracle/product/10.2
    export ORACLE_SID=gzxx
    export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
    export ORACLE_BIN=$ORACLE_HOME/bin/
    export PATH=$PATH:ORACLE_BIN

    #Define variable and parameter
    LOG_DIR=/data/dump_dir
    DUMP_DIR=$LOG_DIR
    FILE_NAME=`date +%Y%m%d%H%M`
    PARA=$1
    ALL_USERS="JCMS2 JCMS1 TYSP_SYSTE TYSP VCGZXX JISGZXX JSEARCHGZXX JGETGZXX JIEPGZ JCMSDOWN JCMS25GZ LM WEBSITE LC JCMSGZ
    XX"

    #expdp funciton
    function Backup (){
        echo "Expdp Start Time: `date` OWNER: $OWNER">>$LOG_DIR/expdp.log
        expdp \'/ as sysdba\' directory=DUMP dumpfile=$OWNER.$FILE_NAME.dump logfile=$OWNER.$FILE_NAME.log schemas=$OWNER
        echo "Expdp Finish Time: `date` OWNER: $OWNER">>$LOG_DIR/expdp.log
    }

    if [ "$PARA" = "all" ];then
        for OWNER  in $ALL_USERS
        do
            Backup
        done
    else
        OWNER=$PARA
        Backup
    fi

    # Delete expired files
    find $LOG_DIR -name "*.dump" -mtime +15 -exec rm {} \;
    find $LOG_DIR -name "*.log" -mtime +15 -exec rm {} \;

#### 1.3 站群数据库

**IP:** 10.10.10.44

**User:** root

**Password:** oracle2014

**backup\_method: **datapump

**backup\_script: **

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

