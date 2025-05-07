docker exec -it 05-mssql-adventuresdbserver-dockermssql-1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "BasesDeDatos234a" -Q "RESTORE DATABASE AdventureWorks FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks.mdf', MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks.ldf', REPLACE;"


