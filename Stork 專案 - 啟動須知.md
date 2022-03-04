---
tags: Stork 開發日誌
---
<i class="fa fa-diamond hightlight" aria-hidden="true"></i>  Stork 專案 - 啟動須知
===


[![hackmd-github-sync-badge](https://hackmd.io/2cgxcyeCRu2YUPPeIMDNpQ/badge)](https://hackmd.io/2cgxcyeCRu2YUPPeIMDNpQ)

>`$` 代表輸入的指令，`#` 代表註解，輸入指令時，`$` 及 `# 後面的文字不用跟著輸入` 


## 第一次 clone 專案下來時，依照下列指令依序建立專案
+ 要先安裝 **postgresql** 
    >MAC: https://www.stevenchang.tw/blog/2019/06/27/Install-PostgreSQL-in-Rails-Project
    >WIN: https://docs.microsoft.com/zh-tw/windows/wsl/tutorials/wsl-database
+ 📌 不要用了 `brew install postgresql` 然後再去 postgresql 下載安裝檔，會發生連接資料庫時錯誤～


>// TODO 安裝文章連結待整理



### **1. 先將專案以 `git clone` 複製下來**

```shell
$ git clone https://github.com/5xDisco/Stork.git # HTTP 協定方式

# or

$ git clone git@github.com:5xDisco/Stork.git  # SSH 協定方式
```
### **2. 使用 `cd stork` 切換到專案 stork 資料夾**
```shell
$ cd stork
```

### **3. bundle 安裝相關後端套件**
```shell
$ bundle install      # 安裝相關後端套件
```

### **4. 安裝相關前端套件**
```shell
$ yarn install
```

### **5. 要建立或寫入資料庫時，注意 postgresql 有沒有啟動**

- **Windows OS - WSL 啟動 postgresql**

```shell
$ sudo service postgresql start 
```

- **Mac OS - 啟動 postgresql**


```shell
$ brew services start postgresql

# 或

$ pg_ctl -D /usr/local/var/postgres start
```

📌 注意！終端機關掉後再打開的情況，PG service 會斷線，要記得重啟。



### 6. `rails db:setup` 
>等於 `db:create` + `db:schema:load` + `db:seed` 
> 👉🏻  [`rails db:setup` 指令說明](https://stackoverflow.com/questions/10301794/difference-between-rake-dbmigrate-dbreset-and-dbschemaload)


```shell
$ rails db:setup  
```



### 7. `foreman start` 
執行 `rails server` 和 `webpack server` ~ 測試一下有沒有問題


```shell
$ foreman s 

# or 

$ foreman start
```

### 8. 開啟瀏覽器, 輸入`http://localhost:3000/`，可以看到以下的預設畫面
>Stork 專案啟動成功

![](https://i.imgur.com/yHsAsvz.png)





---

## 根據上述步驟做完並顯示預設Rails 畫面後，就可以切分支開始工作摟

之後假如有新增 migration 其他人 pull 之後要先
```
$ rails db:migrate
```

假如有新增後端套件，其他人 pull 之後要先

```
$ bundle install
```

假如有新增前端套件，其他人 pull 之後要先

```
$ yarn install
```

---

## Resources

### 1. PG 終端機基本操作參考：

- [\[PSQL\] PostgreSQL CLI | PJCHENder 未整理筆記](https://pjchender.dev/database/psql-cli)
- [使用 WSL 新增或連接資料庫 | Microsoft Docs](https://docs.microsoft.com/zh-tw/windows/wsl/tutorials/wsl-database#install-postgresql)



### 2. 
<!-- TODO -->