# Oracle Database 12c Release 2 \(12.2\) Installation On RHEL 7.X X86-64

**1.Configure hostname and hosts**

```
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.56.21    guiyang.dataforum.org


[root@localhost ~]# cat /etc/hostname 
guiyang.dataforum.org
```

**2.Configure kernel parameters**

```
[root@localhost ~]# cat /etc/sysctl.conf 
# System default settings live in /usr/lib/sysctl.d/00-system.conf.
# To override those settings, enter new settings here, or in an /etc/sysctl.d/<name>.conf file
#
# For more information, see sysctl.conf(5) and sysctl.d(5).
fs.file-max = 6815744
kernel.sem = 250 32000 100 128
kernel.shmmni = 4096
kernel.shmall = 1073741824
kernel.shmmax = 4398046511104
kernel.panic_on_oops = 1
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
net.ipv4.conf.all.rp_filter = 2
net.ipv4.conf.default.rp_filter = 2
fs.aio-max-nr = 1048576
net.ipv4.ip_local_port_range = 9000 65500



[root@localhost ~]# /sbin/sysctl -p
fs.file-max = 6815744
kernel.sem = 250 32000 100 128
kernel.shmmni = 4096
kernel.shmall = 1073741824
kernel.shmmax = 4398046511104
kernel.panic_on_oops = 1
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
net.ipv4.conf.all.rp_filter = 2
net.ipv4.conf.default.rp_filter = 2
fs.aio-max-nr = 1048576
net.ipv4.ip_local_port_range = 9000 65500


[root@localhost ~]# cat /etc/security/limits.d/oracle-database-server-12cR2-preinstall.conf
oracle   soft   nofile    1024
oracle   hard   nofile    65536
oracle   soft   nproc    16384
oracle   hard   nproc    16384
oracle   soft   stack    10240
oracle   hard   stack    32768
oracle   hard   memlock    134217728
oracle   soft   memlock    134217728
```

**3.Install  x86-64 Red Hat Enterprise Linux 7 Minimum Operating System Requirements**

```
binutils-2.23.52.0.1-12.el7 (x86_64)
compat-libcap1-1.10-3.el7 (x86_64)
compat-libstdc++-33-3.2.3-71.el7 (i686)
compat-libstdc++-33-3.2.3-71.el7 (x86_64)
glibc-2.17-36.el7 (i686)
glibc-2.17-36.el7 (x86_64)
glibc-devel-2.17-36.el7 (i686)
glibc-devel-2.17-36.el7 (x86_64)
ksh
libaio-0.3.109-9.el7 (i686)
libaio-0.3.109-9.el7 (x86_64)
libaio-devel-0.3.109-9.el7 (i686)
libaio-devel-0.3.109-9.el7 (x86_64)
libgcc-4.8.2-3.el7 (i686)
libgcc-4.8.2-3.el7 (x86_64)
libstdc++-4.8.2-3.el7 (i686)
libstdc++-4.8.2-3.el7 (x86_64)
libstdc++-devel-4.8.2-3.el7 (i686)
libstdc++-devel-4.8.2-3.el7 (x86_64)
libxcb-1.9-5.el7 (i686)
libxcb-1.9-5.el7 (x86_64)
libX11-1.6.0-2.1.el7 (i686)
libX11-1.6.0-2.1.el7 (x86_64)
libXau-1.0.8-2.1.el7 (i686)
libXau-1.0.8-2.1.el7 (x86_64)
libXi-1.7.2-1.el7 (i686)
libXi-1.7.2-1.el7 (x86_64)
libXtst-1.2.2-1.el7 (i686)
libXtst-1.2.2-1.el7 (x86_64)
make-3.82-19.el7 (x86_64)
net-tools-2.0-0.17.20131004git.el7 (x86_64) (for Oracle RAC and Oracle Clusterware)
nfs-utils-1.3.0-0.21.el7.x86_64 (for Oracle ACFS)
smartmontools-6.2-4.el7 (x86_64)
sysstat-10.1.5-1.el7 (x86_64)
```

**4.Create the new groups and users**

```
groupadd -g 54321 oinstall
groupadd -g 54322 dba
groupadd -g 54323 oper
groupadd -g 54324 backupdba
groupadd -g 54325 dgdba
#groupadd -g 54326 kmdba
#groupadd -g 54327 asmdba
#groupadd -g 54328 asmoper
#groupadd -g 54329 asmadmin
#groupadd -g 54330 racdba

useradd -u 54321 -g oinstall -G dba,oper,backupdba,dgdba oracle


[root@localhost ~]# passwd oracle
Changing password for user oracle.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
```

**5.Addition configuration**

```
[root@localhost ~]# cat /etc/selinux/config 

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=permissive
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 


[root@localhost ~]# setenforce Permissive
```

# Oracle Database 11G Release 2 \(11.2\) Installation On RHEL 6.X X86-64

**1.Configure hostname and hosts**

```
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.56.21    guiyang.dataforum.org


[root@localhost ~]# cat /etc/hostname 
guiyang.dataforum.org
```

**2.Configure kernel parameters**

```
[root@localhost ~]# cat /etc/sysctl.conf 
# System default settings live in /usr/lib/sysctl.d/00-system.conf.
# To override those settings, enter new settings here, or in an /etc/sysctl.d/<name>.conf file
#
# For more information, see sysctl.conf(5) and sysctl.d(5).
fs.suid_dumpable = 1
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 536870912
kernel.shmmni = 4096
# semaphores: semmsl, semmns, semopm, semmni
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default=262144
net.core.rmem_max=4194304
net.core.wmem_default=262144
net.core.wmem_max=1048586



[root@localhost ~]# /sbin/sysctl -p
fs.file-max = 6815744
kernel.sem = 250 32000 100 128
kernel.shmmni = 4096
kernel.shmall = 1073741824
kernel.shmmax = 4398046511104
kernel.panic_on_oops = 1
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
net.ipv4.conf.all.rp_filter = 2
net.ipv4.conf.default.rp_filter = 2
fs.aio-max-nr = 1048576
net.ipv4.ip_local_port_range = 9000 65500


[root@localhost ~]# cat /etc/security/limits.conf
oracle              soft    nproc   16384
oracle              hard    nproc   16384
oracle              soft    nofile  4096
oracle              hard    nofile  65536
oracle              soft    stack   10240
```

**3.Install  x86-64 Red Hat Enterprise Linux 6.X Minimum Operating System Requirements**

```
binutils-2.20.51.0.2-5.11.el6 (x86_64)
compat-libcap1-1.10-1 (x86_64)
compat-libstdc++-33-3.2.3-69.el6 (x86_64)
compat-libstdc++-33-3.2.3-69.el6.i686
gcc-4.4.4-13.el6 (x86_64)
gcc-c++-4.4.4-13.el6 (x86_64)
glibc-2.12-1.7.el6 (i686)
glibc-2.12-1.7.el6 (x86_64)
glibc-devel-2.12-1.7.el6 (x86_64)
glibc-devel-2.12-1.7.el6.i686
ksh
libgcc-4.4.4-13.el6 (i686)
libgcc-4.4.4-13.el6 (x86_64)
libstdc++-4.4.4-13.el6 (x86_64)
libstdc++-4.4.4-13.el6.i686
libstdc++-devel-4.4.4-13.el6 (x86_64)
libstdc++-devel-4.4.4-13.el6.i686
libaio-0.3.107-10.el6 (x86_64)
libaio-0.3.107-10.el6.i686
libaio-devel-0.3.107-10.el6 (x86_64)
libaio-devel-0.3.107-10.el6.i686
make-3.81-19.el6
sysstat-9.0.4-11.el6 (x86_64)




yum install -y binutils-2*x86_64*
yum install -y glibc-2*x86_64* nss-softokn-freebl-3*x86_64*
yum install -y glibc-2*i686* nss-softokn-freebl-3*i686*
yum install -y compat-libstdc++-33*x86_64*
yum install -y glibc-common-2*x86_64*
yum install -y glibc-devel-2*x86_64*
yum install -y glibc-devel-2*i686*
yum install -y glibc-headers-2*x86_64*
yum install -y elfutils-libelf-0*x86_64*
yum install -y elfutils-libelf-devel-0*x86_64*
yum install -y gcc-4*x86_64*
yum install -y gcc-c++-4*x86_64*
yum install -y ksh-*x86_64*
yum install -y libaio-0*x86_64*
yum install -y libaio-devel-0*x86_64*
yum install -y libaio-0*i686*
yum install -y libaio-devel-0*i686*
yum install -y libgcc-4*x86_64*
yum install -y libgcc-4*i686*
yum install -y libstdc++-4*x86_64*
yum install -y libstdc++-4*i686*
yum install -y libstdc++-devel-4*x86_64*
yum install -y make-3.81*x86_64*
yum install -y numactl-devel-2*x86_64*
yum install -y sysstat-9*x86_64*
yum install -y compat-libstdc++-33*i686*
yum install -y compat-libcap*
```

**4.Create the new groups and users**

```
groupadd -g 501 oinstall
groupadd -g 502 dba
groupadd -g 503 oper
groupadd -g 504 asmadmin
groupadd -g 506 asmdba
groupadd -g 505 asmoper

useradd -u 502 -g oinstall -G dba,asmdba,oper oracle
passwd oracle


[root@localhost ~]# passwd oracle
Changing password for user oracle.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
```

**5.Addition configuration**

```
[root@localhost ~]# cat /etc/selinux/config 

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=permissive
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 


[root@localhost ~]# setenforce Permissive
```



