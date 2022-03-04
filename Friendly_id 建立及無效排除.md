---
 tags: 問題和解決
---

# **Friendly_id 建立及無效排除**

## **安裝套件**

```console=
gem 'friendly_id'
```

**終端機：**

```shell=
bundle install

rails g friendly_id
```

建立一個migrate檔、一個config檔。然後記得 **rails db:migrate**

## **設定**
### **Model設定**
在目標網址（:id）的model 加入以下內容（如space.rb）。
其中的 `:name`，代表抓取該當 model 中的 name 欄內容取代原網址，欲自訂網址可依此類推。
```ruby=
  extend FriendlyId
  friendly_id :name, use: :slugged
```

### Controller設定
在目標網址（:id）的controller，將所有 **find** 改為 **friendly.find**

### 增加slug資料欄
若此時執行會有錯誤（undefinded method 'slug'），因為資料表尚無friendly_id必須的「slug」欄位。請以終端機建立之。
* 名字（add_slug_to_spaces）依用途自取，本例是對 Space 增加（還有 Channel 與 User）。
* 預期 slug 產生的陣列內容不重複，故為 uniq。

```shell=
rails g migration add_slug_to_spaces slug:uniq
```

然後記得 **`rails db:migrate`**


## 故障排除
理論上，完成以上設定，即可以指定內容取代部份原網址。

若毫無效果：

1. 將 `friendly_id.rb` 中的「**config.use :finders**」打開。
2. 若原本抓取的欄位無效，就先在Application Record加上自訂方法
```ruby=
def friendly_params
    Digest::SHA1.hexdigest([Time.now, rand].join).truncate(9)
end
```
3. 對應網頁中的連結，原本帶.id的變數要改為`.slug`。

## 擴增
以 Stork 為例，要再擴增至「私聊」功能的網址。

1. 在該當網頁（_slidebar）的對應連結中，將.id改為.slug。
```ruby=
space_direct_message_path(@space.slug, user.slug)
```
2. 解決。

