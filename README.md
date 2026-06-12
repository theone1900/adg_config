
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
 12. 请输入数据库操作系统用户名 [oracle]: 

--------------------------------------------------------------------------------
[2026-06-12 15:30:21] [INFO] 演练模式启用：跳过外部依赖严格性检查（仍建议在真实运行前确认 sqlplus/rman/ssh 可用）
[2026-06-12 15:30:21] [WARN] ==========================================================
[2026-06-12 15:30:21] [WARN]   当前已启用 [-demo] 演练模式！不执行破坏性命令，仅输出剧本。
[2026-06-12 15:30:21] [WARN] ==========================================================
[2026-06-12 15:30:21] [INFO] 完整执行演练剧本将存盘至: C:\Users\ZMI\AppData\Local\Temp\dg_demo_dryrun_orcl_20260612_153021.log
[2026-06-12 15:30:21] [CHECK] 正在执行本地 ORACLE_HOME 路径及主库合规性状态审计...
+------------------------------------------------------------------------------+
  [模拟状态] 主库当前日志模式: [ ARCHIVELOG ]   |   强制日志状态: [ YES ]
+------------------------------------------------------------------------------+
[2026-06-12 15:30:21] [INFO] 【模拟校验通过】主库状态完美符合物理备库架构基线要求。
[2026-06-12 15:30:21] [CHECK] >>> 正在规划主库集群级初始化参数 (SPFILE)...
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set db_unique_name='orcl' scope=spfile;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set log_archive_config='DG_CONFIG=(orcl,orcl_std)' scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_1='LOCATION=/u01/app/oracle/oradata/orcl/arch VALID_FOR=(ONLINE_LOGFILES,ALL_ROLES) DB_UNIQUE_NAME=orcl' scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_2='SERVICE=orcl_std ASYNC VALID_FOR=(ONLINE_LOGFILES,PRIMARY_ROLE) DB_UNIQUE_NAME=orcl_std' scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_state_1=ENABLE scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set log_archive_dest_state_2=ENABLE scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set fal_server='orcl_std' scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set fal_client='orcl' scope=both;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter system set standby_file_management=AUTO scope=both;
[2026-06-12 15:30:21] [CHECK] >>> 正在规划为主库镜像槽注入 4 组 Standby Redo Log 空间...
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 4 '/u01/app/oracle/oradata/orcl/sredo01.log' size 50M;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 5 '/u01/app/oracle/oradata/orcl/sredo02.log' size 50M;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 6 '/u01/app/oracle/oradata/orcl/sredo03.log' size 50M;
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: alter database add standby logfile group 7 '/u01/app/oracle/oradata/orcl/sredo04.log' size 50M;
[2026-06-12 15:30:21] [CHECK] >>> 重启主库以应用 SPFILE 参数配置 (ARCHIVELOG 模式下 shutdown immediate 后 startup)...
[2026-06-12 15:30:21] [INFO] [模拟] 主库执行: shutdown immediate; startup
[2026-06-12 15:30:21] [CHECK] >>> 正在模拟交叉转换并本地生成备库 PFILE 文本媒介...
[2026-06-12 15:30:21] [INFO] [模拟本地SQL执行]: create pfile='/tmp/initorcl_std.ora' from spfile;
[2026-06-12 15:30:21] [INFO] [模拟文本改写] 将 /tmp/initorcl_std.ora 内所有的 orcl 关键字热替换为 orcl_std
[2026-06-12 15:30:21] [CHECK] >>> 正在模拟跨网同步控制链钥匙 (密码文件与重构PFILE) 到备库...
[2026-06-12 15:30:21] [INFO] [模拟本地系统调用]: scp \u01\app\oracle\product\11.2.0\dbhome_1\dbs\orapworcl oracle@192.168.1.20:/u01/app/oracle/product/11.2.0/dbhome_1/dbs/orapworcl
[2026-06-12 15:30:21] [INFO] [模拟本地系统调用]: scp /tmp/initorcl_std.ora oracle@192.168.1.20:/tmp/
[2026-06-12 15:30:21] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 构建备库物理OFA拓扑目录
[2026-06-12 15:30:21] [INFO] 拟派发指令集: 
	mkdir -p /u01/app/oracle/oradata/orcl_std
	mkdir -p /u01/app/oracle/fast_recovery_area/orcl_std
	mkdir -p /u01/app/oracle/oradata/orcl_std/arch
	mkdir -p /u01/app/oracle/admin/orcl_std/adump
[2026-06-12 15:30:21] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 拉起备库裸实例到 NOMOUNT
[2026-06-12 15:30:21] [INFO] 拟派发指令集: 
	sqlplus -s / as sysdba << EOF
	startup nomount pfile='/tmp/initorcl_std.ora';
	create spfile from pfile='/tmp/initorcl_std.ora';
	exit;
	EOF
[2026-06-12 15:30:21] [CHECK] >>> 正在配置备库静态监听以支持 RMAN 远程连接...
[2026-06-12 15:30:21] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 配置备库静态监听和 TNS 配置
[2026-06-12 15:30:21] [INFO] 拟派发指令集: 
	# 由 Go 工具自动生成
	export ORACLE_HOME=/u01/app/oracle/product/11.2.0/dbhome_1
	export ORACLE_SID=orcl_std
	
	# 确保 network/admin 目录存在
	mkdir -p $ORACLE_HOME/network/admin
	
	# 生成或更新 listener.ora 配置文件
	cat > \u01\app\oracle\product\11.2.0\dbhome_1\network\admin\listener.ora << 'LISTENER_EOF'
	LISTENER =
	  (DESCRIPTION_LIST =
	    (DESCRIPTION =
	      (ADDRESS = (PROTOCOL = TCP)(HOST = 0.0.0.0)(PORT = 1521))
	    )
	  )
	
	SID_LIST_LISTENER =
	  (SID_LIST =
	    (SID_DESC =
	      (SID_NAME = orcl_std)
	      (ORACLE_HOME = $ORACLE_HOME)
	    )
	  )
	LISTENER_EOF
	
	# 关键：生成 tnsnames.ora 配置文件（而不是追加）
	# 包含备库本地连接和主库连接信息
	# 这对 RMAN duplicate (target sys/passwd@primary auxiliary /) 至关重要
	cat > $ORACLE_HOME/network/admin/tnsnames.ora << 'TNSNAMES_EOF'
	# 备库本地连接（用于 RMAN auxiliary 连接）
	ORCL_STB =
	  (DESCRIPTION =
	    (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521))
	    (CONNECT_DATA =
	      (SERVICE_NAME = orcl_std)
	    )
	  )
	
	# 主库连接（用于 RMAN target 连接）
	# RMAN duplicate 需要从备库连接到主库以获取数据
	ORCL_PRI =
	  (DESCRIPTION =
	    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.1.10)(PORT = 1521))
	    (CONNECT_DATA =
	      (SERVICE_NAME = orcl)
	    )
	  )
	TNSNAMES_EOF
	
	# 启动或重启监听（如果已运行则先停止）
	lsnrctl stop LISTENER 2>/dev/null || true
	sleep 2
	lsnrctl start LISTENER 2>&1
	
	# 检查监听状态
	sleep 1
	lsnrctl status LISTENER 2>&1
[2026-06-12 15:30:21] [CHECK] >>> 执行网络连通性检查以确保 RMAN duplicate 能够成功...
[2026-06-12 15:30:21] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 验证主备库 TNS 网络连通性
[2026-06-12 15:30:21] [INFO] 拟派发指令集: 
	export ORACLE_HOME=/u01/app/oracle/product/11.2.0/dbhome_1
	export TNS_ADMIN=$ORACLE_HOME/network/admin
	
	# 1. 验证备库配置的 TNS 别名能否解析主库
	echo "验证对主库 'ORCL_PRI' 的 TNS 名称解析..."
	tnsping ORCL_PRI 2>&1 | head -5
	
	# 2. 验证本地连接（备库 loopback）
	echo ""
	echo "验证备库本地连接..."
	sqlplus -s / as sysdba << 'SQLEOF' 2>&1
	set pagesize 0 feedback off verify off heading off echo off
	SELECT 'Connected to Standby: ' || DB_UNIQUE_NAME FROM V$DATABASE;
	SELECT OPEN_CURSORS FROM V$DATABASE;
	exit;
	SQLEOF
	
	echo ""
	echo "网络连通性检查完毕"
[2026-06-12 15:30:21] [CHECK] >>> 正在模拟全面接入 RMAN 联机物理克隆传输通道 (Active Database Duplicate)...
[2026-06-12 15:30:21] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: RMAN 联机物理克隆
[2026-06-12 15:30:21] [INFO] 拟派发指令集: 
	rman target sys/@ORCL_PRI auxiliary / << EOF
	run {
	  allocate channel p1 type disk;
	  allocate channel p2 type disk;
	  allocate auxiliary channel s1 type disk;
	  allocate auxiliary channel s2 type disk;
	  duplicate target database for standby from active database nofilenamecheck;
	}
	EOF
[2026-06-12 15:30:21] [INFO] [模拟远程SSH投递] -> 节点: 192.168.1.20 | 阶段: 备库切入只读模式并强制拉起 MRP0 日志同步进程
[2026-06-12 15:30:21] [INFO] 拟派发指令集: 
	sqlplus -s / as sysdba << EOF
	alter database open;
	alter database recover managed standby database disconnect from session using current logfile;
	exit;
	EOF

================================================================================
[2026-06-12 15:30:21] [INFO] 模拟部署演练指令全部安全留痕完毕！
================================================================================
 模拟 Open Mode 审计结果为: [ READ ONLY WITH APPLY ]

 [SUCCESS] 演练通过！该模拟剧本脚本完全契合全自动安全部署标准。

进程 已完成，退出代码为 0
