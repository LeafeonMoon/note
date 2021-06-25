# MariaDB / MysqlDB
[TOC]

# 1 MariaDB / MysqlDB 安裝

## 1.1 Arch
### 1.1.1 pacman 安裝
```bash
# 安裝
sudo pacman -S mariadb
# 設置服務可用狀態
sudo systemctl enable mariadb.server
# 設置服務開啓，運行前需要執行『2.1』操作
sudo systemctl start mariadb.server
```
## 1.2 Debian
### 1.2.1 apt 安裝
```bash
# 安裝
sudo apt-get install mariadb-server
# 設置服務可用狀態
sudo systemctl enable mariadb@.server
# 設置服務開啓，運行前需要執行『2.1』操作
sudo systemctl start mariadb@.server
```



# 2 初始化數據庫操作

## 2.1 初始化
在啓動 mariadb.service 前需要運行以下命令進行初始化，再啓動 mariadb.service。
```bash
# 數據庫初始化
mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
# 初始化後啓動服務，服務名為「mariadb.server」，也有可能是「mariadb@.server」，具體使用「Tab」鍵展示的服務名，或者直接使用「mariadb」
sudo systemctl enable mariadb.server
sudo systemctl start mariadb.server
```
## 2.2 設置安全選項
設置過程，會初始化 root 密碼。
```bash
mysql_secure_installation
```
## 2.3 升級
```bash
mysql_upgrade -u root -p
```



# 3 登錄和退出

```bash
# 攜帶密碼登錄
# mysql -u「用戶名」 -p「密碼」
mysql -uroot -p123456

# 不攜帶密碼登錄，輸入命令後按「Enter」輸入密碼
# mysql -u「用戶名」 -p
mysql -uroot -p

# 退出mysql控制檯
exit
```



# 4 用戶創建和授權

## 4.1 查詢數據庫用戶信息

```bash
# 登錄，見「3 登錄和退出」
mysql -uroot -p123456
# 使用數據庫「mysql」
use mysql;
# 查詢數據庫用戶信息
select User,Host,Password from user;
```

數據庫「mysql」中的表「user」記錄數據庫用戶信息，

- 字段「User」，數據庫用戶帳號

- 字段「Host」，數據庫用戶訪問IP，「localhost」為本地訪問，「%」為遠程訪問的所有IP，可設定指定IP訪問

- 字段「Password」，數據庫用戶帳號的登錄密碼，已進行加密處理

## 4.2 創建本地用戶

| Name | Description |
| -- | -- |
| userame | 用戶名 |
| passwaord | 密碼 |
```bash
CREATE USER 'userame'@'localhost' IDENTIFIED BY 'passwaord';
```
## 4.3 給予用戶本地訪問權限
| Name | Description |
| -- | -- |
| ALL PRIVILEGES ON *.* | 給予的權限，『*.*』是所有權限 |
| userame | 用戶名 |
| localhost | 本地（訪問） |
```bash
# 8.0 以下
GRANT ALL PRIVILEGES ON *.* TO 'userame'@'localhost' WITH GRANT OPTION;

# 8.0
GRANT ALL PRIVILEGES ON *.* TO 'userame'@'localhost';
```
## 4.4 給予用戶遠程訪問權限
| Name | Description |
| -- | -- |
| ALL PRIVILEGES ON *.* | 給予的權限，『*.*』是所有權限 |
| 'userame'@'%' | username - 用戶名，% - 爲所有外部訪問地址，也可以指定地址 |
| passwaord | 密碼，改密碼爲遠程密碼，與本地登錄密碼不同 |
```bash
# 8.0 以下
GRANT ALL PRIVILEGES ON *.* TO 'userame'@'%' IDENTIFIED BY 'passwaord' WITH GRANT OPTION;

# 8.0，授權和設置密碼是分開的
# 授權
GRANT ALL PRIVILEGES ON *.* TO 'userame'@'%';
# 設置密碼
ALTER USER 'userame'@'%' IDENTIFIED WITH mysql_native_password BY 'passwaord';
```
## 4.5 刷新權限
```bash
FLUSH PRIVILEGES;
```



# 5 備份和恢復数据库

| Name | Description |
| -- | -- |
| -u | 用戶名標識 |
| -p | 密碼標識 |
| -P | 端口標識，默認 3306 |
| username | 數據庫用戶名|
| password | 密碼 |
| dbname | 數據庫名稱 |
| xxx.sql | 數據庫文件位置 |
| container_name:/home | container_name - 容器名稱（id也可），/home - 容器服務器位置 |
## 5.1 備份/導出
```bash
# 攜帶密碼
mysqldump -uusername -ppassword dbname > xxx.sql

# 不攜帶密碼登錄，輸入命令後按「Enter」輸入密碼
mysqldump -uusername -p dbname > xxx.sql
```
## 5.2 恢復/導入
```bash
# 攜帶密碼
mysql -uusername -ppassword dbname < xxx.sql

# 不攜帶密碼登錄，輸入命令後按「Enter」輸入密碼
# 「--default-character-set=utf8」設置編碼防止導入亂碼
# 按「Enter」鍵後輸入數據庫密碼，等待導入操作完成
mysql -uusername -p dbname < xxx.sql --default-character-set=utf8
```



# 6 Docker

## 6.1 安裝MYSQL鏡像

### 6.1.1 拉取MYQL鏡像

```bash
sudo docker pull mysql
```

### 6.1.2 創建MYSQL鏡像

```bash
# sudo run -itd --name「容器名字」 -p 「本機端口:容器端口」 -e MYSQL_ROOT_PASSWORD=「MYSQL管理員root密碼」 「MYSQL鏡像名稱」
# MYSQL鏡像名稱，默認最新版本（mysql:last），可自定義版本
sudo docker run -itd --name mysql-test --restart=always -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

### 6.1.3 進入MYSQL容器（命令模式）

```bash
# sudo exec -it 「鏡像名稱」 /bin/bash
sudo docker exec -it mysql-test /bin/bash
```

## 6.2 操作MYSQL

### 6.2.1 備份/導出

```bash
# docker exec -it 「鏡像名稱」 mysqldump -u「帳號」 -p「密碼」 「數據庫名稱」 > 「SQL文件」
sudo docker exec -it mysql-test mysqldump -uroot -p123456 dbname > dbname.sql
# 僅導出結構，增加參數「-d」
sudo docker exec -it mysql-test mysqldump -uroot -p123456 -d dbname > dbname.sql
```

### 6.2.2 恢復/導入

```bash
# 把SQL文件「dbname.sql」複製到容器「mysql-test」的root目錄下
sudo docker cp dbname.sql mysql-test:/root/
# 進入容器「mysql-test」
sudo docker exec -it mysql-test /bin/bash
# 進入複製好的SQL文件「dbname.sql」的所在位置
cd /root
# mysql -u「帳號」 -p 「數據庫名稱」 < 「SQL文件」
# 「--default-character-set=utf8」設置編碼防止導入亂碼
# 按「Enter」鍵後輸入數據庫密碼，等待導入操作完成
mysql -uroot -p dbname < dbname.sql --default-character-set=utf8
```



## 7 注意問題

### 7.1 Debian
```bash
# 要修改配置文件的 bind-address，默認地址爲 127.0.0.1，把該行代碼註釋
nano /etc/mysql/mariadb.conf.d/50-server.cnf
```