---
tags: 技術資料統整
---

PostgreSQL - pgAdmin 圖形化介面
===


[![hackmd-github-sync-badge](https://hackmd.io/3rYdXIPUQWOH7E953-dbXA/badge)](https://hackmd.io/3rYdXIPUQWOH7E953-dbXA)

:::info
開啟 pgAdmin 的一些設定
:::

#### 在 pgAdmin 中建立 Server
1. General 中 輸入 Name 
    >專案名稱: Stork
![](https://i.imgur.com/orvUG4w.png)

2. Connection 中輸入 Host (name/address)
    >目前本地端: localhost
![](https://i.imgur.com/YmeQ1OA.png)

3. port: 5432
    >default port number
4. Maintenance database
    >default: postgres
5. Username
    >default: postgres 預設會填寫 postgres
6. Password
    >default: empty



####  按下儲存後出現的錯誤
1. [psql: FATAL: role “postgres” does not exist](https://stackoverflow.com/questions/15301826/psql-fatal-role-postgres-does-not-exist)
> Ctrl + F --> "user3402754" 's answer

>e.g. 這是將 Username輸入為test, 出現同樣錯誤的示意圖
![](https://i.imgur.com/yk118TF.png)

在 Terminal 中輸入 建立 Username 為 postgres 的指令: 
```console
/usr/local/opt/postgres/bin/createuser -s postgres
```




---


Resources
---

https://ithelp.ithome.com.tw/articles/10218435






