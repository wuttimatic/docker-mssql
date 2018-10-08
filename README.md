# docker-mssql

1. pull images จาก Microsoft
```
sudo docker pull mcr.microsoft.com/mssql/server:2017-latest
```
2. Run container image ขึ้นมา 
```
sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=password' \
   -p 1433:1433 --name sql2017 \
   -d mcr.microsoft.com/mssql/server:2017-latest
```
3. View container
```
docker ps 
```

## Change SA password
```
sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
   -S localhost -U SA -P '<CurrentPassword>' \
   -Q 'ALTER LOGIN SA WITH PASSWORD="<NewPassword>"'
```

## Connect to SQL Server
```
sudo docker exec -it sql1 "bash"
```

พอเชื่อมต่อ container ได้แล้ว สามารถเชื่อมต่อ database ได้โดย (เนื่องจาก sqlcmd ไม่ได้อยู่ใน path โดย default จึงต้องระบุ path เต็ม)
```
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'password'
```
*** หมายเหตุ เราไม่ต้องใส่ รหัสก็ได้ แต่ระบบจะถามอีกครั้งหลัง run คำสั่ง connect ไป

เมื่อเชื่อมต่อ สำเร็จก็จะได้
```
1>
```

## เชื่อมต่อ database จากภายนอก container
ถ้าต้องการเชื่อต่อด้วย command line สามารถทำได้โดย ติดตั้ง sql-cmd สำหรับ mac osx ใช้ brew 
```
# brew untap microsoft/mssql-preview if you installed the preview version 
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install --no-sandbox mssql-tools
#for silent install: 
#ACCEPT_EULA=y brew install --no-sandbox mssql-tools

```
เมื่ติดตั้งเสร็จแล้วพร้อมเชื่อมต่อ

```
sqlcmd -S 10.3.2.4,1433 -U SA -P 'password'
```

