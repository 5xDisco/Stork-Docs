---
tags: Stork 開發日誌
---

# Space工作空間事宜

## 會員登入
利用devise，可產出current_user。

## CRUD
### Route
可以用resources做出整組CRUD路徑。
非首頁path，在route中建議用as設定路徑名稱。

### Controller
route會找向controller的action。所以：
* 視route設定，終端機（產生器）下指令做出Controller。
* 手工建立也行，比較麻煩。
* 以複數命名
```ruby=
rails g controller spaces

rails g controller pages
```
### Model
controller會與model溝通，所以：
* 先確定model要哪些欄位（column）。
* id欄是預設自帶的。
* 以單數命名
* 欄位屬性為string可不填
```ruby=
rails g model space name icon
```
記得 **rails db:migration** 轉為真正資料表

![](https://i.imgur.com/hKQz5iw.png)

### View 
在 app/views/spaces 下增加 controller 中 def目的的網頁。

## 各實際功能
### 空間列表
**在controller**
* 原本這樣抓「全部」空間列表。
* 設置實體變數，方便view使用。
```ruby=
def list
    @spaces = spaces.all
end
```
* 只抓「目前用戶」之下的空間列表。
```ruby=
def list
    @spaces = current_user.spaces.order(id: :desc)
end
```
**在view**
轉出列表名稱（多數是show）

* 原本只抓名稱
```ruby=
<% @spaces.each do |space| %>
    <%= space.name %>
<% end %>
```

* 結合抓取channel資料表的公開頻道、上傳頭像
* 0, 2是什麼意思？
```ruby=
<% @space_public_channels.each do |mall| %>
    <%= link_to space_channel_path(space_id: mall.space_id, id: mall.id) do %>
        <% if mall.space.avatar.attached? %>
            <%= image_tag mall.space.space_url, alt: "Space's Avatar" %>
        <% else %>
            <%= mall.space.name[0,2]%>
        <% end %>
    <% end %>
<% end %>
```

### 新增
**在view**
放入新增資料的表單（填表），本例為step1（通常是new）
* 本例網頁在pages目錄下，用pages controller給它action，包括實體變數@space。
* class名稱非必要，本例為實施許多設定，故增加。
* 欲填入的欄位「text_field」接「冒號欄位名」。
```htmlembedded=
<%= form_for @space, html: {class:"stepsForm"} do |f| %>
    <div class="step1-text-field-box">
        <%= f.text_field :name, class: "steps-text-field", placeholder: "例如：公司 A 或團隊 B" %>
    </div>
    <div>
        <%= f.label :avatar %>
        <%= f.file_field :avatar %>
        <div class="avatar_display"></div>
          </div>
    <div class="steps-submit-box">
        <%= f.submit "下一步", class: "steps-submit" %>
    </div>
<% end %>
```

**在controller**
1. 用new方法實施新增。
1. route會給new用post方法傳送。
1. 而post會去找create。

因為裝了devise才有current_user用，沒有的話要自己作。
```ruby=
def step1
     @space = current_user.spaces.new
end
```
注意，要**清洗資料**
1. 要設定允許通過的column。
2. 在同一個controller下新增私人方法，用permit允許的欄位參數。
4. 若未清洗，會發生 ActiveModel::ForbiddenAttributesError 錯誤。

```ruby=
private
    def space_params
        params.require(:space).permit(:name, :icon, :user_id)
    end
```

接球，繼續新增create方法。
1. 本例移到了另外一個controller（spaces_controller）。
2. 這裡space_params的位置，本來是承接new拋出的欄位資料（params[:space]），現在改承接「清洗後」的欄位資料。
3. 本來新增失敗，是用 **render :new** 重新填寫（用渲染的，以免重導path讓前面填的資料消失）。本例有特殊情形（多步驟），故用 redirect_to 。
```ruby=
def create
    @space = current_user.spaces.new(space_params)

    if current_user.save
        redirect_to stork_step2_path
    else
        redirect_to stork_step1_path
    end
end
```

### 編輯
**在View**
* 開route路徑表查正確的edit寫法。
```ruby=
<% @spaces.each do |space| %>
    <%= link_to "編輯空間", edit_space_path %>
<% end %>
```

**在Controller**
編輯、刪除都需要抓資料。

* 利用find_by方法，可以把id抓出來，存入實體變數。
* 編輯以PUT或PATCH傳送，會觸發update方法，也要寫上去。
```ruby=
def edit
    @space = current_user.Space.find_by(id: params[:id])
    
    if @space.update(space_params)
        redirect_to space_path, notice: "空間編輯成功"
    else
        render :edit
    end
end
```
在View，放入與新增一樣的form_for就好。它判斷資料為舊，就會認定是要「編輯」。

一樣的部份，可以做成小公版（_檔案名稱.html.erb），放在同一View目錄裡。
* 重複的部份就用局部渲染 <%= render "form", space: @space %>
* 但建議，小公版檔案中，不要用實體變數，都改成區域變數。
* 本例不做，麻煩。

### 刪除
**在View**
1. 開route路徑表查正確的edit連結寫法。
2. 刪除注意要加method。
3. 額外加的data參數可以彈出訊息。
```ruby=
<% @spaces.each do |space| %>
    <%= link_to "編輯空間", edit_space_path %>
    <%= link_to "刪除空間", space_path, method: "delete", data: {confirm: "空間刪除了！"} %>
<% end %>
```

**在controller**
刪除以DELETE傳送，會觸發destroy方法。
```ruby=
def destory
    @space = current_user.Space.find_by(id: params[:id])
    if @space.destroy 
        redirect_to candidates_path, notice: "空間已刪除"
    end
end
```

#### 補充：局部渲染
正確的寫法是 <%= render partial: "form" %>


