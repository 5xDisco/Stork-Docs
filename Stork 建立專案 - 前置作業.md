---
tags: Stork 開發日誌
---

Stork 建立專案 - 前置作業
===

**1. 在 Github 創建一個新的 Repository 準備**

**2. 開一個新的 Rails 專案，預設資料庫使用 postgresql**
>會使 Gemfile 和 config/database.yml 產生相對應的變化

```
$ rails new stork --database=postgresql
```

**3. 創建本地資料庫，產生2個本地資料庫**

```console
$ rails db:create

# database 'Stork_development' 
# database 'Stork_test'
```
>![](https://i.imgur.com/oOKPuKK.png)


**4. 產生 schema.rb**
```console
$ rails db:migrate
```


**5. 然後在 Gemfile 增加 foreman**
>📌 注意！請新增到開發或測試環境下，方便多開 server 並且加快編譯速度

![](https://i.imgur.com/so3IFH0.png)

```ruby
# stork/Gemfile
group :development, :test do
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'foreman', '~> 0.87.2'
end
```




**6. 在終端機換到專案 Stork 資料夾中 ，輸入`bundle install` 安裝 foreman ，他會順便更新 `gemfile.lock`**

```ruby
# $ cd stork
$ bundle install
```

**7. 然後新增 `procfile` 因為 `foreman` 要用，內容如下**

```console
$ mkdir procfile
```
procfile 中輸入
```console
web: rails server -p 3000
webpack: bin/webpack-dev-server
```
![](https://i.imgur.com/MFOoP2S.png)

<!-- Following TODO -->

**8. 然後打開終端機 cd 到此目錄，把 github 遠端倉儲網址加入**

![](https://i.imgur.com/FST3Qcl.png)

- 設置了 main 跟 develop 兩個分支
- 全部東西加一加，寫個 commit 堆到遠端去
- 然後去 github repository 設定一些東西，如圖

![](https://i.imgur.com/FzeBotd.png)

![](https://i.imgur.com/5tutYPr.png)

![](https://i.imgur.com/tXo12zT.png)

- 然後把團隊成員加入可以 push 和 pull

![](https://i.imgur.com/Gwoqfwx.png)

ps: 要用這個的原因是因為 Heroku 部署預設是用 postgresql 也不能用 sqlite3 因為他不支援同時寫入

ps: bundle vs gem install different https://medium.com/lynn-%E7%9A%84%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98/rails-%E6%96%B0%E6%89%8B%E6%9D%91-bundle-install-%E5%92%8C-gem-install-%E7%9A%84%E5%B7%AE%E5%88%A5-bd416ee8b2eb

ps: 順便把這個 rack-mini-profiler 內建觀測網頁的效能東東先註解掉 


[Rails — bundle install 和 gem install 的差別](https://medium.com/lynn-%E7%9A%84%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98/rails-%E6%96%B0%E6%89%8B%E6%9D%91-bundle-install-%E5%92%8C-gem-install-%E7%9A%84%E5%B7%AE%E5%88%A5-bd416ee8b2eb)

---


## Resources 

1. [MS - WSL 安裝 postgresql | 官方指南](https://docs.microsoft.com/zh-tw/windows/wsl/tutorials/wsl-database)
2. [在 Rails 專案安裝 PostgreSQL | From Zen to Code](https://www.stevenchang.tw/blog/2019/06/27/Install-PostgreSQL-in-Rails-Project)