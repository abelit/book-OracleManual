-- DataGuard



-- 1. Primary DB \(orcl\_gy\)



-- 1.1

alter database force logging;



-- 1.2

alter database add standby logfile size 200M;

alter database add standby logfile size 200M;

alter database add standby logfile size 200M;

alter database add standby logfile size 200M;



-- 1.3

alter system set log\_archive\_config='dg\_config=\(orcl\_gy,orcl\_bj\)';

alter system set log\_archive\_dest\_2='service=orcl\_bj async valid\_for=\(online\_logfile,primary\_role\) db\_unique\_name=orcl\_bj';

alter system set standby\_file\_management=AUTO;



-- 1.4

alter database archivelog;



-- 1.5

cat /u01/app/oracle/product/12.2.0.1/db\_1/network/admin/listener.ora

\# listener.ora Network Configuration File: /u01/app/oracle/product/12.2.0.1/db\_1/network/admin/listener.ora

\# Generated by Oracle configuration tools.

SID\_LIST\_LISTENER =

  \(SID\_LIST =

    \(SID\_DESC =

      \(GLOABAL\_NAME = orcl\_gy.dataforum.org\)

      \(ORACLE\_HOME = /u01/app/oracle/product/12.2.0.1/db\_1\)

      \(SID\_NAME = orcl\)

    \)

    \(SID\_DESC =

      \(GLOABAL\_NAME = orcl\_bj.dataforum.org\)

      \(ORACLE\_HOME = /u01/app/oracle/product/12.2.0.1/db\_1\)

      \(SID\_NAME = orcl\)

    \)

  \)



LISTENER =

  \(DESCRIPTION\_LIST =

    \(DESCRIPTION =

      \(ADDRESS = \(PROTOCOL = TCP\)\(HOST = gy.dataforum.org\)\(PORT = 1521\)\)

    \)

  \)



-- 1.6

cat /u01/app/oracle/product/12.2.0.1/db\_1/network/admin/tnsnames.ora

\# tnsnames.ora Network Configuration File: /u01/app/oracle/product/12.2.0.1/db\_1/network/admin/tnsnames.ora

\# Generated by Oracle configuration tools.



PDBORCL =

  \(DESCRIPTION =

    \(ADDRESS\_LIST =

      \(ADDRESS = \(PROTOCOL = TCP\)\(HOST = gy.dataforum.org\)\(PORT = 1521\)\)

    \)

    \(CONNECT\_DATA =

      \(SERVICE\_NAME = pdborcl.org\)

    \)

  \)



ORCLBJ  =

  \(DESCRIPTION =

    \(ADDRESS\_LIST =

      \(ADDRESS = \(PROTOCOL = TCP\)\(HOST = bj.dataforum.org\)\(PORT = 1521\)\)

    \)

    \(CONNECT\_DATA =

      \(SERVICE\_NAME = orcl\)

    \)

  \)



ORCLGY  =

  \(DESCRIPTION =

    \(ADDRESS\_LIST =

      \(ADDRESS = \(PROTOCOL = TCP\)\(HOST = gy.dataforum.org\)\(PORT = 1521\)\)

    \)

    \(CONNECT\_DATA =

      \(SERVICE\_NAME = orcl\)

    \)

  \)



ORCL\_GY =

  \(DESCRIPTION =

    \(ADDRESS\_LIST =

      \(ADDRESS = \(PROTOCOL = TCP\)\(HOST = gy.dataforum.org\)\(PORT = 1521\)\)

    \)

    \(CONNECT\_DATA =

      \(SERVICE\_NAME = orcl\_gy.org\)

    \)

  \)



ORCL\_BJ =

  \(DESCRIPTION =

    \(ADDRESS\_LIST =

      \(ADDRESS = \(PROTOCOL = TCP\)\(HOST = bj.dataforum.org\)\(PORT = 1521\)\)

    \)

    \(CONNECT\_DATA =

      \(SERVICE\_NAME = orcl\_bj.org\)

    \)

  \)



-- 1.7

scp orapworcl oracle@bj.dataforum.org:/u01/app/oracle/product/12.2.0.1/db\_1/dbs/

scp initorcl.ora oracle@bj.dataforum.org:/u01/app/oracle/product/12.2.0.1/db\_1/dbs/



-- 2. Standby DB \(orcl\_bj\)

cat /u01/app/oracle/product/12.2.0.1/db\_1/dbs/initorcl.ora

\*.db\_domain='org'

\*.db\_name='orcl'



startup nomount pfile=/u01/app/oracle/product/12.2.0.1/db\_1/dbs/initorcl.ora

alter system set log\_archive\_dest\_1='location=USE\_DB\_RECOVERY\_FILE\_DEST valid\_for=\(all\_logfiles,all\_roles\) db\_unique\_name=orcl\_gy'



-- 3. Primary DB \(orcl\_gy\)

rman target sys/Chen15285649896@orcl\_gy auxiliary sys/Chen15285649896@orcl\_bj



run {

   allocate channel c1 type disk;

   allocate channel c2 type disk;

   allocate auxiliary channel aux type disk;

  --  allocate auxiliary channel cstby2 type disk;

   duplicate target database for standby from active database spfile

  --  parameter\_value\_convert 'orcl\_gy','orcl\_bj'

   set db\_unique\_name='orcl\_bj'

  --  set fal\_client='orcl\_bj'

  --  set fal\_server='orcl\_gy'

   set standby\_file\_management='AUTO'

   set log\_archive\_config='dg\_config=\(orcl\_gy,orcl\_bj\)'

   set log\_archive\_dest\_1='location=USE\_DB\_RECOVERY\_FILE\_DEST valid\_for=\(all\_logfiles,all\_roles\) db\_unique\_name=orcl\_bj'

   set log\_archive\_dest\_2='service=orcl\_gy ASYNC valid\_for=\(ONLINE\_LOGFILE,PRIMARY\_ROLE\) db\_unique\_name=orcl\_gy'

   ;

}



run {

   allocate channel c1 type disk;

   allocate channel c2 type disk;

   allocate auxiliary channel aux type disk;

   duplicate target database for standby from active database spfile

   set db\_unique\_name='orcl\_bj'

   set standby\_file\_management='AUTO'

   set log\_archive\_config='dg\_config=\(orcl\_gy,orcl\_bj\)'

   set log\_archive\_dest\_1='location=USE\_DB\_RECOVERY\_FILE\_DEST valid\_for=\(all\_logfiles,all\_roles\) db\_unique\_name=orcl\_bj'

   set log\_archive\_dest\_2='service=orcl\_gy ASYNC valid\_for=\(ONLINE\_LOGFILE,PRIMARY\_ROLE\) db\_unique\_name=orcl\_gy'

   ;

}



alter database set standby database to maximize availability;





-- 4. Standby DB \(orcl\_bj\)



-- 备库切换到open状态

-- 退出redo应用状态

-- 停止standby的redo应用 ALTER DATABASE RECOVER MANAGED STANDBY DATABASE CANCEL;注意，此时只是暂时redo 应用，并不是停止Standby 数据库，standby 仍会保持接收只不过不会再应用接收到的归档，直到你再次启动redo 应用为止。类似mysql里面的stop slave功能;

alter database recover managed standby database cancel;

alter database open read only;



-- 再应用redo日志

alter database recover managed standby database using current logfile disconnect;



--去primary、standby库上面执行检查redo或archivelog应用情况

select sequence\#, first\_time, applied from v$archived\_log order by sequence\#;



-- 查看数据库状态

select dbid,name,open\_mode,database\_role,swichover\_status from v$database;







-- 5. 主备切换

-- 5.1 \(Primary DB\)

-- 查看状态

select dbid,name,open\_mode,database\_role,switchover\_status,protection\_mode from v$database;

-- 当switchover\_status为时"to standby"

alter database commit to switchover to physical standby;

-- 当switchover\_status不为"to standby"

alter database commit to switchover to physical standby with session shutdown;

-- 重启数据库

shutdown immediate;

startup nomount;

alter database mount standby database;





-- 5.2 \(Standby DB\)

-- 应用redo

alter database recover managed standby database disconnect from session;

-- 查看状态

select dbid,name,open\_mode,database\_role,switchover\_status,protection\_mode from v$database;

-- 当switchover\_status为时"to primary"

alter database commit to switchover to primary;

-- 当switchover\_status不为"to primary"

alter database commit to switchover to primary with session shutdown;

-- 重启数据库

shutdown immediate;

startup;



