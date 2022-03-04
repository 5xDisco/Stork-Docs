---
tags: 技術資料統整
---
# Action Cable

[![hackmd-github-sync-badge](https://hackmd.io/Qb3_hDhsQJavU-ZQPaekKA/badge)](https://hackmd.io/Qb3_hDhsQJavU-ZQPaekKA)

## 概論
### WebSocket

WebSocket 通訊協定可以讓瀏覽器和伺服器，進行持續性的雙向連線溝通，目前瀏覽器支援度普及。

### Pub/Sub 訂閱模型
Publish–subscribe pattern （發布/訂閱設計模式）是一種在即時通訊上很常用的架構，將通訊分成**發布方和訂閱方**，發布方非同步地將訊息傳送給不定數量的訂閱方。

### Action Cable
Rails 5 開始實裝的 Action Cable，簡單來說就是Pub/Sub 模型 + WebSocket 的 Ruby 框架。整合了後端 Rails 和前端 JavaScript 呼叫介面，可以很方便的開發即時通訊的App。

[Rails 官網](https://guides.rubyonrails.org/action_cable_overview.html)

### 環境設定

#### Step 1 安裝 Redis 並啟動它

安裝：在 Gemfile 把 Redis 的註解拿掉，跑 bundle install

開啟：在終端機執行

WSL
```
sudo service redis-server start
```

Mac
```
brew services start redis
```


#### Step 2 修改 config/cable.yml 檔案 

```
development:
  adapter: redis         #將 async 改成 redis
  url: redis://某個路徑   #將 production 下的路徑複製貼

test:
  adapter: test

production:
  adapter: redis
  url: <%= ENV.fetch("REDIS_URL") { "redis://某個路徑" } %>
  channel_prefix: ac_explorer_production
```

### 簡單實作

Action Cable 和 app/channels 資料夾下的檔案息息相關

首先，我們可以用指令生成相關檔案，如下：

```
rails g channel room  #room 是自訂名稱
```
這行指令可以幫我們產生數個檔案，其中 app/channels/room_channel.rb 和 app/javascript/channels/room_channel.js 是我們的兩大主角，看檔名就知道，它們分別處理前後端。

接下來我們在 room_channel.rb，改寫 subscribed 方法：
```
class RoomChannel < ApplicationCable::Channel
  def subscribed
    stream_from "room_channel"
  end

  def unsubscribed
  end
end
```
然後在 room_channel.js 寫上：
```
connected() {
  console.log("connected to room channel");
},
```
打開 rails server，就可以在瀏覽器後台看到 connected to room channel 了。

上面在做的事，其實從字面上很好理解，就是 room channel **連通**時，執行了　console.log。 以此類推，當要在 channel **接收訊息**時做事，我們可以在 received 中寫下：

```
received(data) {
  console.log(data)  #data是傳進來的值
}
```
然後打開 rails console 模式，向 room channel 傳送訊息：
```
ActionCable.server.broadcast 'room_channel', {message: 'Hello'}
```
我們就可以在瀏覽器後台看到`{message: 'Hello'}`被傳進來了。

以上就是 Action Cable 的簡單實作。





