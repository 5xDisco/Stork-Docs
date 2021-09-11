# devise套件 註冊後redirect路徑 - 如何指定

Devise製作的「註冊後」功能，自動導向首頁。
在ApplicationController寫方法指定登入/出路徑，可能有效，但註冊後路徑不能這樣改：
* def after_sign_in_path_for(resource)
* after_sign_out_path_for(resource_or_scope)

若要更改：

## 第一階段
* 看路徑表，確認Devise給的（註冊功能）的路徑名稱：registration
* 要製作**同名Controller替代原來的**，並在裡面**指定新路徑**
* 在官網得到終端機的安裝指令 
    * **rails generate devise:controllers users -c=registration**
    * 見 https://github.com/heartcombo/devise#configuring-controllers

## 第二階段
* 建立完畢，若原本在Controller有寫 def after_sign_up_path_for 之類的，先移除。
* 將**Devise指向新Controller**，在Route將devise_for  :users 改為 **devise_for :users, controllers: { registrations: 'users/registrations' }**
* 在新的registration controller將 **def after_sign_up_path_for** 打開，加上目標的新路徑，如：
```ruby=
def after_sign_up_path_for(resource)
    stork_step1_path
end
```

###### tags: `問題和解決`