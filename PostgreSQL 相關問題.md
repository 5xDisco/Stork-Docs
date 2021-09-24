---
tags: 問題和解決
---
# PostgreSQL 相關問題

## 建立本地資料庫權限不足
Q：
**rails db:create** 之後發生錯誤「PG::InsufficientPrivilege: ERROR: permission denied to create database Ruby on rails application」

A：
原因是沒有權限建立資料庫。
至psql shell 下執行命令，建立一個使用者即可。
> **ALTER USER 你的使用者名稱 CREATEDB;**

建立途中另有警告訊息，但不影響檔案建立。現有解答無法解決（見參考2）。
![](https://i.imgur.com/iuXfTCI.png)


參考1：https://stackoverflow.com/questions/32571883/pginsufficientprivilege-error-permission-denied-to-create-database-ruby-on-r
參考2：https://stackoverflow.com/questions/45437824/postgresql-warning-could-not-flush-dirty-data-function-not-implemented

## Model實體化的雙重資料表衝突
Q：
* 下完rails g model ~~~ 之後，進行rails db:migrate時發生錯誤「PG::DuplicateTable: ERROR:  relation "該model名稱" already exists」
* 若專案有副本，同時開2個同名model（正/副本）所致。一次用一個就好。


A：
至專案目錄下
> **rails db:drop** //先移除資料表
> **rails db:create** //重建資料表
> **rails db:migrate** //實體化

![](https://i.imgur.com/KrTe35T.png)




