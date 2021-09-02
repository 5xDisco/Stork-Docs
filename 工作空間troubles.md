# 工作空間troubles

## 登入登出問題
* 看不到devise生成的helper或controller設定內容。
* 因devise已在route中生成路徑devise_for :users。這造成登入後會自動跳回首頁，但必須改跳到自訂的網頁（工作空間列表）
    * 解決方式：在routes設定，見 https://stackoverflow.com/questions/7638920/rails-redirect-after-sign-in-with-devise
>       namespace :user do
    >       root :to => "spaces#list"
>       end
* 承上，也須設定「註冊後」跳到new_space_path（新增工作空間），暫未成功
    https://stackoverflow.com/questions/4987841/devise-redirect-after-sign-up
    https://stackoverflow.com/questions/8003347/overriding-devise-after-sign-up-path-for-not-working
* 登出成功，但就逗留原地。
    * 解決方式：在routes設定(現無效)，見 https://stackoverflow.com/questions/6514421/redirecting-devise-after-sign-out
>def after_sign_out_path_for(resource_or_scope)
>request.referrer
>end
* 一度登出刪除session後，也不能在layout放<%= current_user.email %>了（登入前會錯誤）。* 
* 找到　<%= current_user.email %>　可以印出登入的email帳號，但是從哪的helper或controller生成的？這裡不能用龍哥上課的範例　<%= print_out_session %>　是少寫了什麼。
* 遺忘密碼寄信功能的　ArgumentError in Devise::Passwords#create　錯誤
> 查 devise action mailer
    
### 區別瀏覽/增改權
* 拿到號碼牌session才能登入，但是我看不到devise的這個過程。
* 限定特定頁面登入才能用（新增／編輯／刪除自己的工作空間）， before_action :check_login!加在哪個controller找不到（同上一條）。若打開appliction_controller打開before_action :authenticate_user!，會讓未登入/登出後的 <%= current_user.email %>錯誤。
    * 自訂controller，在routes加設定，未成功  devise_for :users :controllers: { :sessions => 'users' }
    https://stackoverflow.com/questions/6234045/how-do-you-access-devise-controllers
* 區分不出user的權限，目前不同user登入都看到一樣的list。
    * 有關FK user has_many而 space belongs_to的建立？
* 如何確認current_user（還是其他名稱）存在，並指定給另外的欄（做成實體變數之類? 給created_by）

## 資料表問題

* created_by帶入登入的users.id
* icon如何與特定Space綁定

## 邀請會員
* 建立space_member資料表
* 多對多關係/FK　應為？
space_member的 "user_id" belongs_to users.id
space_member的 "space_id" belongs_to spaces.id

* 龍哥的例子是把動態產生卡片addToCart，但這裡應該是把users.id 傳入spaces.id ？再研究


###### tags: `Stork 開發日誌`


