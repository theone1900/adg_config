 ____  ____    _    ____ _     _____    _    ____   ____      ____  _____ _____ _   _ ____  
================================================================================
               欢迎使用 Oracle 11g Active Data Guard Go 自动化部署工具 v1.0
================================================================================
 是否开启 [-demo] 模拟演练模式？(y/n) [y]: 
 1. 请输入主备库的 ORACLE_SID [orcl]: 
 2. 请输入主库 ORACLE_HOME 路径 [/u01/app/oracle/product/11.2.0/dbhome_1]: 
 3. 请输入备库 ORACLE_HOME 路径 [/u01/app/oracle/product/11.2.0/dbhome_1]: 
 4. 请输入主库(Primary) IP [192.168.1.10]: 
 5. 请输入备库(Standby) IP [192.168.1.20]: 
 5. 请输入主库 DB_UNIQUE_NAME [orcl]: 
 6. 请输入备库 DB_UNIQUE_NAME [orcl_std]: 
 7. 请输入主库 TNS 别名 [ORCL_PRI]: 
 8. 请输入备库 TNS 别名 [ORCL_STB]: 
 9. 请输入数据库 SYS 用户的密码: 
 10. 请输入备库 SSH 用户名 [oracle]: 
 11. 请输入备库 SSH 用户密码 (root/oracle): 

--------------------------------------------------------------------------------
[2026-06-08 17:26:48] [WARN] ==========================================================
[2026-06-08 17:26:48] [WARN]   当前已启用 [-demo] 演练模式！不执行破坏性命令，仅输出剧本。
[2026-06-08 17:26:48] [WARN] ==========================================================
[2026-06-08 17:26:48] [INFO] 完整执行演练剧本将存盘至: /tmp/dg_demo_dryrun_orcl_20260608_172648.log
[2026-06-08 17:26:48] [CHECK] 正在执行本地 ORACLE_HOME 路径及主库合规性状态审计...
+------------------------------------------------------------------------------+
  [模拟状态] 主库当前日志模式: [ ARCHIVELOG ]   |   强制日志状态: [ YES ]
+------------------------------------------------------------------------------+
[2026-06-08 17:26:48] [INFO] 【模拟校验通过】主库状态完美符合物理备库架构基线要求。
[2026-06-08 17:26:48] [CHECK] >>> 正在规划主库集群级初始化参数 (SPFILE)...
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set db_unique_name='orcl' scope=spfile;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set log_archive_config='DG_CONFIG=(orcl,orcl_std)' scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_1='LOCATION=/u01/app/oracle/oradata/orcl/arch VALID_FOR=(ONLINE_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=orcl' scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_2='SERVICE=orcl_std ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=orcl_std' scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_state_1=ENABLE scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_state_2=ENABLE scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set fal_server='orcl_std' scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set fal_client='orcl' scope=both;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter system set standby_file_management=AUTO scope=both;
[2026-06-08 17:26:48] [CHECK] >>> 正在规划为主库镜像槽注入 4 组 Standby Redo Log 空间...
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 4 '/u01/app/oracle/oradata/orcl/sredo01.log' size 50M;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 5 '/u01/app/oracle/oradata/orcl/sredo02.log' size 50M;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 6 '/u01/app/oracle/oradata/orcl/sredo03.log' size 50M;
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 7 '/u01/app/oracle/oradata/orcl/sredo04.log' size 50M;
[2026-06-08 17:26:48] [CHECK] >>> 正在模拟交叉转换并本地生成备库 PFILE 文本媒介...
[2026-06-08 17:26:48] [INFO] [模拟本地SQL执行]: create pfile='/tmp/initorcl_std.ora' from spfile;
[2026-06-08 17:26:48] [INFO] [模拟文本改写] 将 /tmp/initorcl_std.ora 内所有的 orcl 关键字热替换为 orcl_std
[2026-06-08 17:26:48] [CHECK] >>> 正在模拟跨网同步控制链钥匙 (密码文件与重构PFILE) 到备库...
[2026-06-08 17:26:48] [INFO] [模拟本地系统调用]: scp \u01\app\oracle\product\11.2.0\dbhome_1\dbs\orapworcl oracle@192.168.1.20:/u01/app/oracle/product/11.2.0/dbhome_1/dbs/orapworcl
[2026-06-08 17:26:48] [INFO] [模拟本地系统调用]: scp /tmp/initorcl_std.ora oracle@192.168.1.20:/tmp/
[2026-06-08 17:26:48] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 构建备库物理OFA拓扑目录
[2026-06-08 17:26:48] [INFO] 拟派发指令集: 
	mkdir -p /u01/app/oracle/oradata/orcl_std
	mkdir -p /u01/app/oracle/fast_recovery_area/orcl_std
	mkdir -p /u01/app/oracle/oradata/orcl/arch
	mkdir -p /u01/app/oracle/admin/orcl/adump
[2026-06-08 17:26:48] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 拉起备库裸实例到 NOMOUNT
[2026-06-08 17:26:48] [INFO] 拟派发指令集: 
	sqlplus -s / as sysdba << EOF
	startup nomount pfile='/tmp/initorcl_std.ora';
	create spfile from pfile='/tmp/initorcl_std.ora';
	exit;
	EOF
[2026-06-08 17:26:48] [CHECK] >>> 正在模拟全面接入 RMAN 联机物理克隆传输通道 (Active Database Duplicate)...
[2026-06-08 17:26:48] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: RMAN 联机物理克隆
[2026-06-08 17:26:48] [INFO] 拟派发指令集: 
	rman target sys/@ORCL_PRI auxiliary / << EOF
	run {
	  allocate channel p1 type disk;
	  allocate channel p2 type disk;
	  allocate auxiliary channel s1 type disk;
	  allocate auxiliary channel s2 type disk;
	  duplicate target database for standby from active database nofilenamecheck;
	}
	EOF
[2026-06-08 17:26:48] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 备库切入只读模式并强制拉起 MRP0 日志同步进程
[2026-06-08 17:26:48] [INFO] 拟派发指令集: 
	sqlplus -s / as sysdba << EOF
	alter database open;
	alter database recover managed standby database disconnect from session using current logfile;
	exit;
	EOF

================================================================================
[2026-06-08 17:26:48] [INFO] 模拟部署演练指令全部安全留痕完毕！
================================================================================
 模拟 Open Mode 审计结果为: [ READ ONLY WITH APPLY ]

 [SUCCESS] 演练通过！该模拟剧本脚本完全契合全自动安全部署标准。

进程 已完成，退出代码为 0
