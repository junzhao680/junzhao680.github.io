# MySQL 連接出現 Authentication plugin 'caching_sha2_password' cannot be loaded

很多用戶在使用Navicat Premium 12連接MySQL數據庫時會出現Authentication plugin ‘caching_sha2_password‘ cannot be loaded的錯誤，解決方法如下

## 1.管理員權限運行命令提示符，登陸MySQL（記得添加環境變量）

mysql -u root -p

password: #登入mysql

![技術分享圖片](http://image.bubuko.com/info/201811/20181102222644011476.png)

## 2.修改賬戶密碼加密規則並更新用戶密碼

ALTER USER ‘root‘@‘localhost‘ IDENTIFIED BY ‘password‘ PASSWORD EXPIRE NEVER; #修改加密規則

ALTER USER ‘root‘@‘localhost‘ IDENTIFIED WITH mysql_native_password BY ‘password‘; #更新一下用戶的密碼



## 3.刷新權限並重置密碼

FLUSH PRIVILEGES; #刷新權限

上面兩步對應的截圖

![技術分享圖片](http://image.bubuko.com/info/201811/20181102222644265392.png)

單獨重置密碼命令：alter user ‘root‘@‘localhost‘ identified by ‘111111‘;



現在再次打開Navicat Premium 12連接MySQL問題數據庫就會發現可以連接成功了

