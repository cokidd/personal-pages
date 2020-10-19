+++
author = "Cokidd"
title = "LDAP(Lightweight Directory Access Protocol 轻量级目录服务)"
date = "2020-10-19"
description = "Guide to LDAP usage"
tags = [
    "Linux",
]

categories = [
    "系统管理员",
]

+++


# LDAP(Lightweight Directory Access Protocol 轻量级目录服务)



## 一、概述

### 1.  目录

   * 目录：专门设计用于搜索和浏览的树状结构数据库
   * 目录特点：

     * 倾向于包含基于属性的描述性信息，并支持复杂的过滤

     * 不支持复杂交互和复杂更新、回滚，通常更新是全有或全无的更改

     * 对大量查找和搜索操作通常会快速响应
     
       

### 2. 目录服务

   * 目录服务：以树状结构数据库为基础，提供相应的信息查询服务
   
     

### 3. LDAP (Lightweight Directory Access Protocol 轻量级目录服务)

   * LDAP:它是用于访问目录服务（特别是基于X.500的目录服务）的轻量级协议。 LDAP通过TCP/IP或其他面向连接的传输服务运行。
   * 数据类型表达：

     * 条目(dn)：条目是具有全局唯一专有名称(DN)属性的集合，DN用于明确地引用条目，每个条目都有一个类型及单一或多个值。
     * 模式(Schema):对象类(ObjectClass)、属性类型(AttributeType)、属性语法(Syntax)和匹配规则(MatchingRules)的集合。
   * 存储信息方式：将条目以树状的层次进行排列
   * LDAP功能：查询条目、更新条目、删除条目、添加条目
   * LDAP适用范围：需要集中管理、存储、访问数据时
   * LDAP架构：Client/Server架构
   * LDAP交互过程：客户端连接到服务端询问，服务器提供应答或对应指针(指针指向从何处可以获得对应信息，类似DNS)

[^DN]: DN为Distinguished Name,表示专有名称。再系统中显示为DN

[^DN类型]: 类型通常为辅助记忆字符串，如cn表示通用名，mail表示邮件地址。值的类型取决于属性类型，如mail属性可能包含值example@game2sky.com；jpegPhoto可能包含值example.jpeg(二进制格式图片)图片
[^Schema模式]: LDAP 协议定义了一些标准的模式，也可以根据需求自定义模式。(模板路径：**/etc/openldap/schema/**)


[^树形命名方式]:以属性特征相同较多的条目位于根附近的位置，以属性特征不同较多的条目为于远离根的位置。如代表国家组织的条目，下面代表着可能是省、市、县等单位或其他类型的单位。
[^internet命名方式]:以属性特征不同较多的条目位于根附近的位置，以属性特征相同较多的条目位于远离根的位置。如域名net\com\DE，在com下方包含组织类型的内容，组织类型的内容下方组织类型单元。(目前公司需要的应该是这一种)



### 4. OpenLDAP(开源LDAP应用)

   * OpenLDAP服务：核心服务为slapd

   * slapd服务特性：

     * 使用LDAPv3协议

     * 简单身份验证和安全层

     * 传输层安全(TLS或SSL)

     * 拓扑控制

     * 访问控制

     * 数据库后端选择

     * 多个数据实例

     * 通用模块api

     * 多线程

     * 复制

     * 代理缓存
     

### 5.OpenLDAP功能汇总

* 实现账号统一集中管理
* 权限控制管理(sudo)
* 密码控制策略管理
* 密码审计管理
* 密码控制策略
* 主机控制管理
* 同步机制管理
* TLS/SASL加密传输
* 高可用负载均衡
* 自定义schema
* 各种应用平台集成账号管理

### 6.OpenLDAP条目概述

* objectClass分类
  * 结构型(structural): 如person和organizationUnit
  * 辅助型(auxiliary):如extensibleObject
  * 抽象型(abstract):如top,抽象型的objectClass不能直接使用

### 7.OpenLDAP属性概述

* 属性(Attribute):在目录树中主要用于描述条目相关信息。例如用户条目的用途、联系方式、邮件、uid、gid、公司地址等
* 常用属性类型:
  * dn(distinguished name):唯一标识名，类似于Linux文件系统中的绝对路径，每个对象都有唯一标识名。(例如，uid=dpgdy, ou=People, dc=gdy, dc=com)
  * rdn(relative dn):通常指相对标识名，类似与Linux文件系统中的相对路径。
  * ui(user id):通常指一个用户的登录名称
  * sn(sur name):通常指一个人的姓氏
  * giveName:通常指一个人的名字
  * I:通常指一个地方的地名。
  * objectClass:objectClass是特殊的属性，包含数据存储的方式以及相关属性信息
  * dc(domain component):通常指定一个域名
  * ou(organization unit):通常指定一个组织单元的名称
  * cn(common name):通常指一个对象的名称，如果是人,需要使用全名
  * mail:通常指登录账号的邮箱地址
  * telephoneNumber:通常指登录账号的手机号码
  * c：通常指一个二位国家的名称

### 8.OpenLDAP管理命令汇总

* ldapsearch:搜索OpenLDAP目录树条目
* ldapadd:通过LDIF格式，添加目录树条目
* ldapdelete:删除OpenLDAP目录树条目
* ldapmodify:修改OpenLDAP目录树条目
* ldapwhoami:检验OpenLDAP目录树条目
* ldapmodrdn:修改OpenLDAP目录树条目
* ldapcompare:判断DN值和指定参数值是否属于同一个条目
* ldappasswd:修改OpenLDAP目录树用户条目实现密码重置
* slaptest:验证slapd.conf文件或cn=配置目录(slapd.d)
* slapindex:创建OpenLDAP目录树索引，提高查询效率
* slapcat:将数据条目转换为OpenLDAP的LDIF文件

### 9.OpenLDAP 同步机制概述

* syncrepl同步提供一个有状态的复制，拉(pull)和推(push)模式进行目录树的同步
* 多并发连接时，数据同步信息不完整需要重新加载ldap进程
* 日志需要分割、清理和同步

### 10.OpenLDAP 权限内容概述

* Linux-PAM 组织架构

  * Linux通过PAM模块来验证登录者合法性及密码正确性

    ![image-20201016154519079](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201016154519079.png)

* PAM模块认证类型

  * auth:认证管理，验证使用者身份，账号和密码
  * accout:用户认证，基于用户表
  * password:密码口令，认证管理
  * session:会话管理，日志记录或则限制用户登录次数

* PAM模块的模块流程

  * required:必要条件，需要同其他模块一起验证。验证失败，则返回失败信息
  * requisite:必要条件，验证失败后即结束整个验证过程
  * sufficient:充分条件，验证成功即返回OK,不再继续验证
  * optional:可选条件，无论验证如何不会影响会话
  * include：包含另外一个配置文件中类型相同的行

* PAM模块

  * pam_rootok.so:如果UID=0,RETURN OK
  * pam_access.so:配置文件为/etc/security/access.conf,登录模块
  * pam_listfile.so:基于文件允许或拒绝访问资源机制
  * pam_time.so:基于时间的访问控制
  * pam_tally2.so:登录统计
  * pam_limits.so:限制会话过程中对资源的使用情况，配置文件/etc/security/limits.conf


11.密码策略属性详解

* pwdAllowUserChange,允许用户修改密码
* pwdAttribute, pwdPolicy对象的一个属性，用于标识用户密码
* pwdExpireWarning,密码过期前警告天数
* pwdFailureCountInterval，密码失败后恢复时间
* pwdGraceAuthNLimit,密码过期后不能登录的天数，0代表禁止登录
* pwdInHistory，开启密码历史记录，用于保证不能和之前设置的密码相同
* pwdLockout,超过定义次数，账号被锁定
* pwdLockoutDuration，密码连续输入错误次数后，账号锁定时间
* pwdMaxAge,密码有效期，到期需要强制修改密码
* pwdMaxFailure,密码最大失效次数，超过后账号被锁定
* pwdMinAge，密码有效期
* pwdMinLength，用户修改密码是最短的密码长度
* pwdMustChange，用户登录系统提示修改密码
* pwdSafeModify，是否允许用户修改密码
* pwdLockoutDuration，账号锁定后，不能自动解锁，需要管理员解锁

## 二、安装和基本配置（CentOS 7)

### 1. 查看是否存在openssl

   ```shell
   [root@localhost ~]# rpm -qa|grep openssl
   #如果内容为空
   [root@localhost ~]# yum -y install openssl
   ```

### 2. 安装openldap

   ```shell
   [root@localhost ~]# yum -y install openldap-servers openldap-clients
   #复制数据库配置
   [root@localhost ~]# cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
   #修改文件权限
   [root@localhost ~]# chown ldap. /var/lib/ldap/DB_CONFIG
   #启动服务
   [root@localhost ~]#systemctl start slapd
   [root@localhost ~]# systemctl enable slapd
   ```

### 3.时间同步设置

* OpenLDAP需要时间进行同步

  ```shell
  ntpdate 0.rhel.pool.ntp.org
  rpm -qa| grep openldap-server
  ```


### 4. 基础配置

   ```shell
   #设置管理员密码
   #生成加密密码
   [root@localhost ~]# slappasswd
   New password:
   Re-enter new password:
   {SSHA}QECj5ITzHfYsmpNvsGUIYZXqTYxIukXw
   #将{SSHA}QECj5ITzHfYsmpNvsGUIYZXqTYxIukXw内容复制
   
   #创建配置文件
   [root@localhost ~]# vi chrootpw.ldif
   #将{SSHA}QECj5ITzHfYsmpNvsGUIYZXqTYxIukXw内容复制至olcRootPW中
   dn: olcDatabase={0}config,cn=config
   changetype: modify
   add: olcRootPW
   olcRootPW: {SSHA}QECj5ITzHfYsmpNvsGUIYZXqTYxIukXw
   
   #添加配置文件(使用SASL机制)
   [root@localhost ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f chrootpw.ldif
   SASL/EXTERNAL authentication started
   SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
   SASL SSF: 0
   modifying entry "olcDatabase={0}config,cn=config"
   
   #导入基本配置
   [root@localhost ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
   SASL/EXTERNAL authentication started
   SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
   SASL SSF: 0
   adding new entry "cn=cosine,cn=schema,cn=config"
   
   [root@localhost ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
   SASL/EXTERNAL authentication started
   SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
   SASL SSF: 0
   adding new entry "cn=nis,cn=schema,cn=config"
   
   [root@localhost ~]# ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
   SASL/EXTERNAL authentication started
   SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
   SASL SSF: 0
   adding new entry "cn=inetorgperson,cn=schema,cn=config"
   
   #导入所有模式
   [root@localhost ~]# find /etc/openldap/schema/ -name "*.ldif" -exec ldapadd -Y EXTERNAL -H ldapi:/// -D "cn=config" -f {} \ 
   
   #设置目录管理员密码(内容不相同)
   #生成加密密码
   [root@localhost ~]# slappasswd
   New password:
   Re-enter new password:
   {SSHA}AgeHfglDlqbccpFV4RLY42eXO4+kM0eD
   #管理目录配置文件
   [root@localhost ~]# vi chdomain.ldif
   # 将dc=***，dc=***内容替换为设置的域名
   # 将{SSHA}AgeHfglDlqbccpFV4RLY42eXO4+kM0eD内容复制至olcRootPW中
   dn: olcDatabase={1}monitor,cn=config
   changetype: modify
   replace: olcAccess
   olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
     read by dn.base="cn=Manager,dc=srv,dc=com" read by * none
   
   dn: olcDatabase={2}hdb,cn=config
   changetype: modify
   replace: olcSuffix
   olcSuffix: dc=srv,dc=com
   
   dn: olcDatabase={2}hdb,cn=config
   changetype: modify
   replace: olcRootDN
   olcRootDN: cn=Manager,dc=srv,dc=com
   
   dn: olcDatabase={2}hdb,cn=config
   changetype: modify
   add: olcRootPW
   olcRootPW: {SSHA}AgeHfglDlqbccpFV4RLY42eXO4+kM0eD
   
   dn: olcDatabase={2}hdb,cn=config
   changetype: modify
   add: olcAccess
   olcAccess: {0}to attrs=userPassword,shadowLastChange by
     dn="cn=Manager,dc=srv,dc=com" write by anonymous auth by self write by * none
   olcAccess: {1}to dn.base="" by * read
   olcAccess: {2}to * by dn="cn=Manager,dc=srv,dc=com" write by * read
   #配置文件添加
   [root@localhost ~]# ldapmodify -Y EXTERNAL -H ldapi:/// -f chdomain.ldif
   SASL/EXTERNAL authentication started
   SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
   SASL SSF: 0
   modifying entry "olcDatabase={1}monitor,cn=config"
   
   modifying entry "olcDatabase={2}hdb,cn=config"
   
   modifying entry "olcDatabase={2}hdb,cn=config"
   
   modifying entry "olcDatabase={2}hdb,cn=config"
   
   #基础域名配置文件
   [root@localhost ~]# vi basedomain.ldif
   #将dc=***，dc=***内容替换为设置的域名
   dn: dc=game2sky,dc=com
   objectClass: top
   objectClass: dcObject
   objectclass: organization
   o: game2sky com
   dc: game2sky
   
   dn: cn=Manager,dc=game2sky,dc=com
   objectClass: organizationalRole
   cn: Manager
   description: Directory Manager
   
   dn: ou=People,dc=game2sky,dc=com
   objectClass: organizationalUnit
   ou: People
   
   dn: ou=Group,dc=game2sky,dc=com
   objectClass: organizationalUnit
   ou: Group
   
   #数据结构添加
   [root@localhost ~]# ldapadd -x -D cn=Manager,dc=game2sky,dc=com -W -f basedomain.ldif
   Enter LDAP Password:     # directory manager's password
   adding new entry "dc=game2sky,dc=com"
   
   adding new entry "cn=Manager,dc=game2sky,dc=com"
   
   adding new entry "ou=People,dc=game2sky,dc=com"
   
   adding new entry "ou=Group,dc=game2sky,dc=com"
   
   ```

### 5. phpLDAPadmin前端页面部署

   ```shell
   [root@localhost ~]# yum --enablerepo=epel -y install phpldapadmin
   [root@localhost ~]# vi /etc/phpldapadmin/config.php
   # 397行取消注释, 398行添加注释
   $servers->setValue('login','attr','dn');
   // $servers->setValue('login','attr','uid');
   [root@localhost ~]# vi /etc/httpd/conf.d/phpldapadmin.conf
   Alias /phpldapadmin /usr/share/phpldapadmin/htdocs
   Alias /ldapadmin /usr/share/phpldapadmin/htdocs
   <Directory /usr/share/phpldapadmin/htdocs>
     <IfModule mod_authz_core.c>
       # Apache 2.4
       Require local
       # 添加Require ip 172.16.0.0/22
       Require ip 172.16.0.0/22
   [root@localhost ~]# systemctl restart httpd
   ```

* 通过web端访问url:ipaddress/ldapadmin/,显示如下

     ![image-20201010160328443](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201010160328443.png)

###    	6. 防火墙配置

* iptables明文数据传输配置:

  ```shell
  iptables -A INPUT -p tcp -m state -state NEW -dport 389 -j ACCEPT
  iptables -A OUTPUT -p tcp -m state -state NEW -dport 389 -j ACCEPT
  service iptables save
  service iptables restart&&chkconfig iptables on 
  ```

* 密文数据传输配置:

  ```shell
  iptables -A INPUT -p tcp -m state -state NEW -dport 636 -j ACCEPT
  iptables -A OUTPUT -p tcp -m state -state NEW -dport 636 -j ACCEPT
  service iptables save
  service iptables restart&&chkconfig iptables on 
  ```

### 7.域名解析设置

  ```shell
  [root@localhost ~]# cat >> /etc/hosts <<EOF
  > ipaddress  hostname firstname
  > EOF
  # ipaddress 表示IP地址
  # hostname 表示服务器域名全称
  # firstname 表示服务器域名第一个名称
  ```

### 8.LDAP客户端部署

* 配置文件功能

  * /etc/nsswitch.conf：用于名称转化服务，用于UNIX&Linux系统验证用户身份所读取本地文件或是远程验证服务器
  * /etc/sysconfig/authconfig：用于提供身份验证值LDAP功能
  * /etc/pam.d/system-auth：主要用于显示现实用户账号身份验证
  * /etc/pam_ldap.conf：用于实现用户账号身份验证
  * /etc/openldap/ldap.conf：主要用于查询OpenLDAP服务器所有条目信息

* 部署方式

  * 图形化部署方式

    * 常用工具setup 、authconfig-tui命令
  
* 配置文件部署方式
  * 命令行部署方式

    * 常用工具authconfig命令
  
* 部署前时间同步
  
  * ntpdate 0.rhel.pool.ntp.org
  * 部署前域名解析
  * vim /etc/hosts
    * 172.16.3.180 openldap.game2sky.com
  
* 安装客户端工具
      ```shell
  yum -y install openldap-clients nss-pam-ldapd authconfig --enableldap \
  --enableldapauth \
  --ldapserver=172.16.3.180 \
  --ldapbasedn="dc=game,dc=com" \
  --enablemkhomedir \
  --update
      ```
  
  

### 9.OpenSSL加密传输设置

* 创建CA证书

```shell
rpm -qa | grep openssl
cd /etc/pki/CA
openssl req -new -x509 -key private/cakey.pem -out cacert.pem 
#设置证书
Country Name(2 lettercode):cn(两个字母的国家代号)
State or Province Name(fullname):Beijing(省份或地区名)
Locality Name(eg, city)[Default City]:市或地区
Organization Name(eg, company)[Default Company Ltd]:公司名称
Organizational Unit Name(eg, section)[]:部门名称
Common Name(eg, your name or your server's hostname)：OL服务器名称
Email Address []: ca@game2sky.com

```

* 生成文件用途
  * cacert.pem:CA 自身证书文件
  * certs:客户端证书存放目录
  * crl:CA吊销的客户端证书存放目录
  * newcerts:生成新证书存放目录
  * index.txt:存放客户端证书信息
  * serial:客户端证书编号
  * private:存放CA自身私钥的目录

* 查看根证书

  ```shell
  openssl x509 -noout -text -in /etc/pki/CA/cacert.pem
  ```

* 服务器生成密钥

* openssl genrsa -out ldapkey.pem 1024

* OpenLDAP服务向CA申请证书签署请求

  ```shell
  openssl req -new -key ldapkey.pem -out ldap.csr -days 3650
  ```

* CA核实并签发证书

  ```shell
  openssl ca -in ldap.csr -out ldapcert.pem -days 3650
  ```

* OpenLDAP  TLS/SASL

  * 修改证书权限

  * chown -R ldap:ldap /etc/openldap/ssl/*

  * chmod -R 0400 /etc/openldap/ssl/*

  * 修改OpenLDAP配置文件

    ```shell
    vim mod_ssl.ldif
    dn: cn=config
    changetype: modify
    add: olcTLSCACertificateFile
    olcTLSCACertificateFile: /etc/openldap/certs/ca-bundle.crt
    -
    replace: olcTLSCertificateFile
    olcTLSCertificateFile: /etc/openldap/certs/server.crt
    -
    replace: olcTLSCertificateKeyFile
    olcTLSCertificateKeyFile: /etc/openldap/certs/server.key
    ldapmodify -Y EXTERNAL -H ldapi:/// -f mod_ssl.ldif
    vim /etc/sysconfig/slapd
    SLAPD_URLS="ldapi:/// ldap:/// ldaps:///"
    systemctl restart slapd
    ```

  * 设置客户端

    ```shell
    echo "TLS_REQCERT allow" >> /etc/openldap/ldap.conf
    echo "tls_reqcert allow" >> /etc/nslcd.conf
    authconfig --enableldaptls --updates
    ```

  * 证书验证

    ```shell
    openssl verify -CAfile /etc/pki/CA/cacert.pem /etc/openldap/ssl/ldapcert.pem
    openssl s_client -connect openldap.game2sky.com:636 -showcerts -state -CAfile /etc/openldap/ssl/cacert.pem
    ```

    


## 三、配置详解

### 1.全局配置类型

* 本地目录服务:不会以任何方式和其他服务器交互

  ![image-20201010171521577](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201010171521577.png)

* 具有引用的本地服务目录(主从模式)：为本地服务提供应用，并将其配置项其他能够处理请求的服务器返回引用。

  ![image-20201010171926713](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201010171926713.png)

* 复制目录服务:syncrepl主服务器提供服务，由多个从服务器通过syncrepl返回数据。![image-20201010172208583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201010172208583.png)

* 分布式本地模式：将本地服务解耦划分为更小的服务，每个服务都可以进行复制，并与上下级引用粘合在一起

### 2.配置树

![image-20201010161811243](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201010161811243.png)	

* slapd配置树:树的根名为cn=config包含全局配置设置，其他设置包含在单独的子条目中。
* 动态加载模块
* 模式定义:cn=schema,cn=config条目包含系统架构。cn=schema,cn=config的子条目包含从配置文件加载或在运行时添加的用户架构
* 后端特定配置
* 数据库特定配置:覆盖在数据库项的子项中定义。数据库和覆盖也可能有其他的子对象

### 3.LDIF配置文件

* LDIF用途:LDIF(LDAP Data Interchanged Format)的轻量级目录访问协议数据交换格式，是存储LDAP配置信息及目录内容的标准文本文件格式。
* LDIF文件特点
  * LDIF 文件每行的结尾不允许有空格或则制表符
  * LDIF文件允许相关属性可以重复赋值并使用
  * LDIF文件以.ldif结尾命名
  * LDIF文件中以#号开头的一行为注释，可以作为解释使用
  * LDIF文件所有的赋值方式为: 属性:[空格]属性值
  * LDIF文件通过空行来定义一个条目，空格前为一个条目，空格后为另一个条目的开始

* 全局配置规则

  ```shell
  dn:cn=config 
  objectClass:olcGlobal 
  cn:config<global config settings>
  ```



* 架构定义(模块定义)

  ```shell
  dn: cn=schema,cn=config 
  objectClass: olcSchemaConfig 
  cn: schema
  ```

* 系统架构(系统模块)

  ```shell
  dn: cn={X}core,cn=schema,cn=config objectClass: olcSchemaConfig cn: {X}core <core schema>
  ```


* 其他用户指定的架构

* 后端定义

  ```shell
  olcBackend=<typeA>,cn=config 
  objectClass: olcBackendConfig 
  olcBackend: <typeA> <backend-specific settings>
  ```

* 数据库定义

  ```shell
   dn: olcDatabase={X}<typeA>,cn=config objectClass: olcDatabaseConfig olcDatabase: {X}<typeA> <database-specific settings>
  ```

[^数字索引]: {x}数字索引x表示顺序依赖关系，LDAP数据库本身是无序的。使用数字索引进行强制排序，以便保留顺序依赖关系。

[^ocl]: 前缀为ocl（OpenLDAP配置），后缀为属性和对象类名称并与slapd.conf文件配置关键字一一对应。

[^配置指令]:配置指令可以使用参数，参数之间使用空格分开。如果一个参数包含空格使用双引号"arg"。上面应该用参数替换的内容为<>中的内容。

* 配置项

  * cn=config
    
    * config设置适用于整个服务器，设置用于系统或面向连接（必须有olcGlobal、objectClass设置才可以使用
    
  * olcldletimeout: <interger>
    
    * 等待(<interger>表示为具体秒数)时间，强制断开与空闲用户的连接
  
* olcLogLevel: <level>
  
  * 系统记录日志的级别，olcLogLevel -l显示具体级别
  
  * olcReferral <URL>
    
    * 当slapd找不到本地数据库来处理请求时要传递的引用
  
* 例示:
  
    ``` shell
    dn: cn=config
    objectClass: olcGlobal
    cn: config
    olcIdleTimeout: 30
    olcLogLevel: Stats
    olcReferral: ldap://root.openldap.org
    ```

  * cn=module

    * 动态模块配置，cn=module条目设置指定要加载的模块集。(必须有olcModuleList、objectClass设置)

  * olcModuleLoad:<filename>

    * 指定要加载的动态可加载模块的名称,<filename>可以是文件名也可以是绝对路径名(文件名在olcModulePath的指定目录中检索)
  
  * olcModuleLoad:<pathspec>
    
    * 需要加载模块所在路径，不同的路径使用冒号分隔(不同操作系统不同)
    
  * 例示: 

  	``` shell
 	 	dn: cn=module{0},cn=config
   	 	objectClass: olcModuleList
    	cn: module{0}
    	olcModuleLoad: /usr/local/lib/smbk5pwd.la
    	dn: cn=module{1},cn=config
    	objectClass: olcModuleList
    	cn: module{1}
    	olcModulePath: /usr/local/lib:/usr/local/lib/slapd
    	olcModuleLoad: accesslog.la
    	olcModuleLoad: pcache.la
    ```

	* cn=schema

  	* schema模板配置,该条目由slapd服务生成（必须有olcSchemaConfig, 			objectClass)

	* olcAttributeTypes: <RFC4512 Attribute Type Description>

  	* 设置属性类型:查看schema 规范设置

	* olcObjectClasses: **<RFC4512 Object Class Description>**

  	* 设置对象类:查看schema 规范设置

	* 例示:

    ```shell
    dn: cn=schema,cn=config
    objectClass: olcSchemaConfig
    cn: schema
    dn: cn=test,cn=schema,cn=config
    objectClass: olcSchemaConfig
    cn: test
    olcAttributeTypes: ( 1.1.1
     NAME 'testAttr'
     EQUALITY integerMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 )olcAttributeTypes: ( 1.1.2 NAME 'testTwo' EQUALITY caseIgnoreMatchSUBSTR caseIgnoreSubstringsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.44 )
    olcObjectClasses: ( 1.1.3 NAME 'testObject' MAY ( testAttr $ testTwo ) AUXILIARY )
    ```


* 后端数据库配置

  * olcBackend: <type>

    * <type>表示后端类型设置

  * 例示

    ```shell
     dn: olcBackend=bdb,cn=config
     objectClass: olcBackendConfig
     olcBackend: bdb
    ```


* 数据库实例设置

  * olcDatabase:[{index}]<type>
    
    * {index}实例数据库索引，<type>为后端数据库类型
    
  *  olcAccess: to <what> [ by <who> [<accesslevel>] [<control>] ]+
    
    * 设置条目的访问权限(<accesslevel> 访问级别、<who>请求者)。默认所有用户
    
  * olc Readonly {TRUE|FALSE}

    * TRUE设置数据库只读
    
  * olcRootDN: <DN>

    * 该指令指定不受访问控制或管理限制的条目对数据库进行操作，条目可以是SASL身份
    * 例示
      * Entry-base "olcRootDN: cn=Manager,dc=example,dc=com"
      * SASL-based "olcRootDN: uid=root,cn=example.com,cn=digest-md5,cn=auth"
    
  * olcRootPW: <password>
    * 修改指定条目的密码
      * olcRootPW: secret
      * 使用hash设置，olcRootPW: {SSHA}ZKKuqbEKJfKSXhUbHG3fG8MDn9j1v4QN
    
  * olcSizeLimit: <integer>
    
    * 设置从搜索区域显示最大条目数
    
  * olcSuffix: <dn suffix>

    * 查询将传递后端数据库查询的DN后缀

    * 例示:olcSuffix: dc=example,dc=com

* olcSyncrepl设置

  * 设置内容：

    ```shell
    olcSyncrepl: rid=<replica ID>
     provider=ldap[s]://<hostname>[:port]
     [type=refreshOnly|refreshAndPersist]
     [interval=dd:hh:mm:ss]
     [retry=[<retry interval> <# of retries>]+]
     searchbase=<base DN>
     [filter=<filter str>]
     [scope=sub|one|base]
     [attrs=<attr list>]
     [attrsonly]
     [sizelimit=<limit>]
     [timelimit=<limit>]
     [schemachecking=on|off]
     [bindmethod=simple|sasl]
     [binddn=<DN>]
     [saslmech=<mech>]
     [authcid=<identity>]
     [authzid=<identity>]
     [credentials=<passwd>]
     [realm=<realm>]
     [secprops=<properties>]
     [starttls=yes|critical]
     [tls_cert=<file>]
     [tls_key=<file>]
     [tls_cacert=<file>]
     [tls_cacertdir=<path>]
     [tls_reqcert=never|allow|try|demand]
     [tls_cipher_suite=<ciphers>]
     [tls_crlcheck=none|peer|all]
     [logbase=<base DN>]
     [logfilter=<filter str>]
     [syncdata=default|accesslog|changelog]
    ```


  * 配置详细内容查看 RFC4533

## 四、命令详解

### 1.  ldapsearch命令

   * 通过用户定义的查询条件，对OpenLDAP目录树进行查找以及检索目录树相关条目
     * -b <searchbase>:查找指定的节点
      * -D<dinddn>:查找指定的DN
      * -v:详细输出信息
      * -x:使用简单的认证，不是用任何加密算法
      * -W:在查询时，会提示输入密码，如不想输入密码，使用-w password
      * -h:使用指定ldaphost，可以使用FQDN或IP地址(FQDN表示可以连通的主机名(/etc/hosts))
      * -H(LDAP-URI):使用LDAP服务器的URI地址进行操作
      * -p(port):指定OpenLDAP监听的端口(默认端口为389，加密端口为636)
      * -LLL:禁止输入与过滤条件不匹配的信息
      * uid：过滤条件
      * cn、gidNumber:将目标信息进一步过滤，显示出相关cn及gidNumber信息
     
        * 示例
      * ldapsearch -x -D"cn=Manager,dc=game2sky,dc=com" -H ldap:/// -b "ou=people,dc=game2sky,dc=com" -W
   * ldapserach -x -LLL -b dc=game2sky,dc=com 'uid=Guodayong' cn gidNumber

### 2. ldapadd命令

   * ldapadd:用于通过LDIF格式添加目录树条目。ldapadd为ldapmodify软连接，功能相当于ldapmodify -a
   
   * 示例
   
     * ```shell
       ldapadd -x -D "cn=Manager,dc=game2sky,dc=com" -W -h 127.0.0.1 -f base.ldif
       ```
```shell
cat <<  EOF|ldapadd -x -D cn=Manager, dc=game2sky,dc=com -W -H ldap:///
> dn: uid=tlin,ou=people,dc=game2sky,dc=com
> changetype: delete
> EOF
```

### 3. ldapdelete命令

   * ldapdelete 命令用于从目录树中删除指定条目，并根据DN条目删除一个或多个条目，但必须提供所要删除指定条目的权限所绑定的DN(整个目录树的唯一标识名称)

     * -c:持续操作模式，操作过程中出现错误，也会进行后续相关操作
     * -D:同上
     * -n:显示正在进行的相关操作，但不实际修改数据，一般用于测试
     * -x:同上
     * -f:使用目标文件名作为命令的输入
     * -W:同上
     * -w passwd:passwd(管理员密码)
     * -y passwdfile:密码文件认证
     * -r:递归删除,从目录树删除指定的DN的所有子条目
     * -h:同上

   * 示例

     * ldapdelete -D "cn=Manager,dc=game2sky,dc=com" -W -h host_ipaddress -x 

     * 删除用户和用户组

     * ldapdelete -x -w gdy@123! -D 'cn=Manager,dc=game2sky,dc=com' "uid=user1,ou=people,dc=game2sky,dc=com"

       

### 4. ldapmodify命令

   * ldapmodify命令可以对OpenLDAP数据库中的条目进行修改操作，可以理解为编辑器。

     * -a:新增条目
     * -D:同上
     * -n:同上
     * -v:详细输出结果
     * -f:同上
     * -w passwd：同上
     * -W:同上
     * -h:同上
     * -H:同上
     * -p:同上

   * 示例

     * ldapmodify -x -D "cn=Manager,cd=game2sky,cd=com" -w gdy@123 -H ldap:/// -f modify.ldif

     * 对系统用户密码进行解锁

     * cat << EOF | ldapmodify -x -H ldap:/// -D cn=config -w gdy@123

     * dn :uid=lisi,ou=people,dc=game2sky,dc=com

     * changetype=modify

     * delete: pwdAccountLockedTime

     * EOF

     * 重置系统用户密码

     * cat << EOF|ldapmodify -x -H ldap:/// -D cn=config -w gdy@123!

     * dn: cn=default,ou=pwpolicies,dc=game2sky,dc=com

     * changetype: modify

     * replace: pwdMustChange

     * pwdMustChange: TRUE

     * EOF

### 5. ldapwhoami命令

   * ldapwhoami用于验证OpenLDAP服务器身份
   * 示例
     * ldapwhoami -x -D "uid=dpgdy,ou=people,dc=game2sky,dc=com" -W -h ipaddress
     * 输入dpgdy密码


### 6. ldapmodrdn命令

   * ldapmodrdn命令用于对OpenLDAP目录树中RDN条目的修改，可以从标准的条目信息输入或使用-f指定ldif文件输入
     * -c: 同上
     * -D:同上
     * -n: 同上
     * -x:同上
     * -f:同上
     * -W:同上
     * -w passwd:
     * -y passwdfile:
     * -r:删除OpenLDAP目录树中rdn(相对路径)条目的唯一标识名称
     * -h:同上
     * -H:同上
     * -p:同上
     * -P:输入OpenLDAP版本号，V2或V3
     * -O props:指定SASL安全属性
   * 示例
     * ldapmodrdn -x -D "cn=Manager,dc=game2sky,dc=com" -W -h ipaddress "uid=dpgdy,ou=people,dc=game2sky,dc=com" uid=Guodayong
     * cat << EOF |ldapmodify -x -D "cn=Manager,dc=game2sky,dc=com" -W -h ipaddress
     * dn: uid=Guodayong,ou=people,dc=game2sky,dc=com
     * changetype:modify
     * delete: uid
     * uid: dpgdy
     * EOF

### 7. ldapcompare命令

   * ldapcompare命令用于判断OpenLDAP目录树中DN值和指定条目是否属于同一条目

     * -D:同上
     * -n:同上
     * -x:同上
     * -W:同上
     * -w passwd:同上
     * -y passwdfile:同上
     * -h:同上
     * -H:同上
     * -p:同上
     * -P:同上

   * 示例

     * ldapcompare -x -D "cn=Manager,dc=game2sky,dc=com" -W -h "uid=dpgdy,ou=people,dc=game2sky,dc=com" "uid:ada"

     * 内容不匹配显示FALSE,内容匹配显示TRUE

       

### 8. ldappasswd命令

   * ldappasswd命令用户修改密码

   * -S:提示用户输入密码

   * -s newPasswd:指定密码(明文提示)

   * -a oldPasswd:通过旧密码生成新密码

   * -A:提示输入旧密码，自动生成新密码

   * -x:使用简单的认证，不使用任何加密的算法

   * -D:同上

   * -W:同上

   * -w passwd:同上

   * 使用DLAP管理员进行用户密码重置

   * ldappasswd -x -D "cn=Manager,dc=game2sky,dc=com" -W "uid=lisi,ou=people,dc=gdy,dc=com" -S

   * 输入用户新密码

   * 使用DLAP管理员进行用户密码旧密码重置

   * ldappasswd -x -D "cn=Manager,dc=game2sky,dc=com" -A -W "uid=zhangsan,ou=people,dc=game2sky,dc=com" -S

   * Oldpassword: 输入旧密码

   * Re-enter old password:确认旧密码

   * New password:输入新密码

   * Re-enter new password:确认新密码

     

### 9. slaptest命令

   * slaptest 命令用于检测配置文件(/etc/openldap/slapd.conf)以及数据文件的可用性
     * -d:指定debug的级别
     * -f:检测指定配置文件/etc/openldap/slapd.conf
     * -F:检测指定的数据库目录/etc/openldap/slapd/
   * 示例
     * slaptest -f /etc/openldap/slapd.conf
     * 检测数据库文件可用性以及定义debug级别，可以使用一下命令
     * slaptest -d 3 -F /etc/openldap/slapd.d/

### 10. slapindex命令

  * slapindex用于创建OpenLDAP数据库条目索引，用于提高查询速度。
  * -f:指定OpenLDAP的配置文件，并创建索引
  * -F:检测指定OpenLDAP数据库目录，并创建索引

### 11. slapcat命令

* slapcat命令用于将数据条目转换为OpenLDAP的LDIF文件，可用于OpenLDAP条目的备份
* -a filter:添加过滤选项
     * -b suffix:指定suffix路径，如dc=game2sky,dc=com
     * -d number: 指定debug输出信息的级别
     * -f:指定OpenLDAP的配置文件
     * -F:指定OpenLDAP的数据库文件目录
     * -c:出现错误信息时，继续输出条目
     * -H:同上
     * -v:输出详细信息
     * 示例
          * slapcat -v -l openldap.ldif




## 五、 高级配置

1.Linux 用户权限管理

* 使用密码

* ldap Server端操作

  ```shell
  #新增用户
  useradd username
  #新增密码
  slappasswd
  输入用户密码生成{SSHA}密码
  #添加用户
  vi ldapuser.ldif
  # create new
  # replace to your own domain name for "dc=***,dc=***" section
  dn: uid=Username,ou=People,dc=game2sky,dc=com
  objectClass: inetOrgPerson
  objectClass: posixAccount
  objectClass: shadowAccount
  cn: Username
  sn: Linux
  userPassword: {SSHA}xxxxxxxxxxxxxxxxx
  loginShell: /bin/bash
  uidNumber: 1000
  gidNumber: 1000
  homeDirectory: /home/Username
  
  dn: cn=Username,ou=Group,dc=game2sky,dc=com
  objectClass: posixGroup
  cn: Username
  gidNumber: 1000
  memberUid: username
  
  ldapadd -x -D cn=Manager,dc=game2sky,dc=com -W -f ldapuser.ldif
  #批量安装脚本
   vi ldapuser.sh
  # extract local users and groups who have 1000-9999 digit UID
  # replace "SUFFIX=***" to your own domain name
  # this is an example
  #!/bin/bash
  
  SUFFIX='dc=srv,dc=world'
  LDIF='ldapuser.ldif'
  
  echo -n > $LDIF
  GROUP_IDS=()
  grep "x:[1-9][0-9][0-9][0-9]:" /etc/passwd | (while read TARGET_USER
  do
      USER_ID="$(echo "$TARGET_USER" | cut -d':' -f1)"
  
      USER_NAME="$(echo "$TARGET_USER" | cut -d':' -f5 | cut -d' ' -f1,2)"
      [ ! "$USER_NAME" ] && USER_NAME="$USER_ID"
  
      LDAP_SN="$(echo "$USER_NAME" | cut -d' ' -f2)"
      [ ! "$LDAP_SN" ] && LDAP_SN="$USER_NAME"
  
      LASTCHANGE_FLAG="$(grep "${USER_ID}:" /etc/shadow | cut -d':' -f3)"
      [ ! "$LASTCHANGE_FLAG" ] && LASTCHANGE_FLAG="0"
  
      SHADOW_FLAG="$(grep "${USER_ID}:" /etc/shadow | cut -d':' -f9)"
      [ ! "$SHADOW_FLAG" ] && SHADOW_FLAG="0"
  
      GROUP_ID="$(echo "$TARGET_USER" | cut -d':' -f4)"
      [ ! "$(echo "${GROUP_IDS[@]}" | grep "$GROUP_ID")" ] && GROUP_IDS=("${GROUP_IDS[@]}" "$GROUP_ID")
  
      echo "dn: uid=$USER_ID,ou=People,$SUFFIX" >> $LDIF
      echo "objectClass: inetOrgPerson" >> $LDIF
      echo "objectClass: posixAccount" >> $LDIF
      echo "objectClass: shadowAccount" >> $LDIF
      echo "sn: $LDAP_SN" >> $LDIF
      echo "givenName: $(echo "$USER_NAME" | awk '{print $1}')" >> $LDIF
      echo "cn: $USER_NAME" >> $LDIF
      echo "displayName: $USER_NAME" >> $LDIF
      echo "uidNumber: $(echo "$TARGET_USER" | cut -d':' -f3)" >> $LDIF
      echo "gidNumber: $(echo "$TARGET_USER" | cut -d':' -f4)" >> $LDIF
      echo "userPassword: {crypt}$(grep "${USER_ID}:" /etc/shadow | cut -d':' -f2)" >> $LDIF
      echo "gecos: $USER_NAME" >> $LDIF
      echo "loginShell: $(echo "$TARGET_USER" | cut -d':' -f7)" >> $LDIF
      echo "homeDirectory: $(echo "$TARGET_USER" | cut -d':' -f6)" >> $LDIF
      echo "shadowExpire: $(passwd -S "$USER_ID" | awk '{print $7}')" >> $LDIF
      echo "shadowFlag: $SHADOW_FLAG" >> $LDIF
      echo "shadowWarning: $(passwd -S "$USER_ID" | awk '{print $6}')" >> $LDIF
      echo "shadowMin: $(passwd -S "$USER_ID" | awk '{print $4}')" >> $LDIF
      echo "shadowMax: $(passwd -S "$USER_ID" | awk '{print $5}')" >> $LDIF
      echo "shadowLastChange: $LASTCHANGE_FLAG" >> $LDIF
      echo >> $LDIF
  done
  
  for TARGET_GROUP_ID in "${GROUP_IDS[@]}"
  do
      LDAP_CN="$(grep ":${TARGET_GROUP_ID}:" /etc/group | cut -d':' -f1)"
  
      echo "dn: cn=$LDAP_CN,ou=Group,$SUFFIX" >> $LDIF
      echo "objectClass: posixGroup" >> $LDIF
      echo "cn: $LDAP_CN" >> $LDIF
      echo "gidNumber: $TARGET_GROUP_ID" >> $LDIF
  
      for MEMBER_UID in $(grep ":${TARGET_GROUP_ID}:" /etc/passwd | cut -d':' -f1,3)
      do
          UID_NUM=$(echo "$MEMBER_UID" | cut -d':' -f2)
          [ $UID_NUM -ge 1000 -a $UID_NUM -le 9999 ] && echo "memberUid: $(echo "$MEMBER_UID" | cut -d':' -f1)" >> $LDIF
      done
      echo >> $LDIF
  done
  )
  
  ```


2.Linux 远程权限设置

* sudo设置

  * 客户端配置

  ```shell
  authconfig --enableldap --enableldapauth -enablemkhomedir\
  --enableforcelegacy --disablessd --disablesssdauth --disableldaptls\
  --enablelocauthorize \
  --ldapserver=ipaddress_server --ldapbasedn="dc=***,dc=***" --enableshadow\
  --update
  #修改nsswitch.conf配置文件，
  cp /etc/nsswitch.conf /etc/nsswitch.conf.bak
  echo "sudoers: files ldap" >> /etc/nsswitch.conf
  #修改/etc/ldap.conf文件设置
  SUDOERS_BASE ou=People,dc=game2sky.com
  #修改sudoer权限
  #或
  gpasswd -a username wheel
  
  
  ```


3.设置密码策略组

* vim pwp.ldif

  ```shell
  dn:ou=pwpolicies,dc=game2sky,dc=com
  objectClass:organizationalUnit
  ou:pwpolices
  
  dn: cn=default,ou=pwpolicies,dc=game2sky,dc=com
  cn: default
  objectClass: pwdPolicy
  objectClass: person
  pwdAllowUserChange: TRUE
  pwdAttribute: userPassword
  pwdExpireWarning:259200
  pwdFailureCountInterval:0
  pwdGraceAuthNLimit: 5
  pwdInHistory: 5
  pwdLockout:TRUE
  pwdLockoutDuration:300
  pwdMaxAge: 259200
  pwdMaxFailure:5
  pwdMinAge:0
  pwdMinLength:8
  pwdMustChange:TRUE
  pwdSafeModify:TRUE
  sn: dummy value
  ```

  * 添加用户使用密码策略

  ```shell
    dn: uid=wulei,dc=game2sky,dc=com
    objectClass: inetOrgPerson
    uid:wulei
    cn:wu lei
    sn:lei
    loginShell: /bin/bash
    homeDirectory: /home/wulei
    homePhone:
    employeeNumber:21378
    mail:wulei@game2sky.com
    pwdPolicySubentry:cn=security,ou=policy,dc=game2sky,dc=com
    
  ```
  
* 定义用户登录修改密码

* vim pwdret.ldif

  ```shell
  #该用户登录密码重置
  dn：uid=dpcwc,ou=people,dc=game2sky,dc=com
  changetype:modify
  replace:pwdReset
  pwdReset:TRUE
  #查看
  ldapsearch -x -LLL uid=dpcwc +
  ```

* 客户端配置

  ```shell
  echo "bind_policy soft">>/etc/pam_ldap.conf
  echo "pam_password md5">>/etc/pam_ldap.conf
  echo "pam_lookup_policy yes">>/etc/pam_ldap.conf
  echo "pam_password clear_remove_old">>/etc/pam_ldap.conf
  重启nslcd服务
  service nslcd restart
  
  ```

  

