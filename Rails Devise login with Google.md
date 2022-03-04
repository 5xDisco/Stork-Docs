---
tags: 技術資料統整
---

# Rails Devise login with Google(Omniauth-google-oauth2) - 未完


1. `git pull` 專案後，打開 `config/application.yml` 檔案中填入帳號＆金鑰
>`config/application.example.yml` 僅做範例，不填寫帳號與金鑰

```console
google_client_id: ""       
google_client_secret: ""    

```
![](https://i.imgur.com/IauyqTw.png)



2. 將`application.yml` 加入 `.gitignore` 檔案中， `git add .` 會自動忽略此檔案，不會commit 上傳。

```console
# Ignore application configuration
/config/application.yml

```


### References
- [Rails實作第三方登入-Google](https://medium.com/tingyiiii/rails%E5%AF%A6%E4%BD%9C%E7%AC%AC%E4%B8%89%E6%96%B9%E7%99%BB%E5%85%A5-google-2a0851b74193)
