---
tags: 問題和解決
---
 
# devise 套件安裝 - Routing Error
>版本：devise 4.8.0
>[devise | RubyGems.org | Ruby 社群 Gem 套件管理平台](https://rubygems.org/gems/devise/versions/4.8.0)



### References：

- [Rails實作devise套件](https://medium.com/annielin28815/rails%E5%AF%A6%E4%BD%9Cdevise%E5%A5%97%E4%BB%B6-38119a7558a5)

---

### 遇到的問題

- `http://127.0.0.1:3000`顯示路徑錯誤

```console=
#Routing Error
uninitialized constant HomeController
```
![](https://i.imgur.com/pNOHXGr.png)



### 解決方式：

**1. 查看 `config/routes.rb` 有沒有設路徑**
>`config/routes.rb`

```ruby=
    Rails.application.routes.draw do 
        devise_for :users
        root to: "home#index"
    end
```

![](https://i.imgur.com/oVBIco0.png)



**2. 查看`app/controller` 之中有沒有對應路徑`homes_controller.rb` 檔案**
>`app/controller/homes_controller.rb`

```ruby=
   class HomesController < ApplicationController
       def index
       
       end
   end
```

![](https://i.imgur.com/iOmaJMt.png)


**3. 查看 `app/views` 之中有沒有對應的`homes/index.html.erb` 檔案**
>`app/views/homes/index.html.erb`

![](https://i.imgur.com/YtcicLB.png)




---

# 成果畫面

- 註冊畫面

![](https://i.imgur.com/AT8R9KT.png)


- 登入成功畫面

![](https://i.imgur.com/mH8r912.png)


- 忘記密碼畫面 （驗證信功能尚未開發）

![](https://i.imgur.com/UXAxxx3.png)



