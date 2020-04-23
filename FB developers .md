# FB developers Place

## 以範圍搜尋
Get : 
```
https://graph.facebook.com/search?type=place&center={center}&distance={distance}&categories={categories}&q={q}&fields={fields}
```
### 參數

-	一定要包含 q 或 center

|name		|說明|
|---		|---|
|center	|一個座標|
|distance	|距中心的距離（以米為單位）|
|fields	|回傳你想要的 **PlaceInformation** 資料。ex : `name,checkins,cover,link`|
|q			|你想要搜尋的名稱， name 中包含此字串|

##	以ID搜尋詳細資料
Get : 
```
https://graph.facebook.com/v5.0/{place-information-id}?fields={list-of-fields}
```

|name|說明|
|---|---|
|id		|地點的 id。|
|about	|關於此地點的訊息。|
|app_links|用 FB APP 開啟此地點的頁面（android、ios）。|
| category_list |此地點的子類別。|
| checkins |此地點的打卡數。|
| cover |封面照片。|
| description ||
| engagement |按讚數。純數字 `count` ex : 100、字串`social_sentence ` ex : "100人說這個讚。"|
| hours |每日營業時間的列表。key屬性被格式化為 {day}\_{number}\_{status}，{day} 星期幾，{number} 允許 1 或 2 兩套不同的運行時間對於給定的一天（例如，早晨和晚上運行時間），{status}是 open 或 close 是開門或關門，value 是一天中的小時，格式為24小時制。ex : key": "mon\_1\_open","value": "11:00"|
| is\_always\_open |是不是總是開放。bool|
| is\_permanently\_closed |是不是永久關閉。bool|
| is\_verified |是不是被 FB 驗證（藍勾勾）。bool|
| link |此地點的 FB 頁面 URL。|
| location |此地點的 location 資訊。|
| name |此地點的名稱。|
| overall\_star\_rating |此地點在 1-5 顆星的評價，來自用戶的自主評價。|
| page |跟此地點有關的頁面。暫時不能用|
| parking |此地點關於停車的資訊，lot 是否有提供停車場，street 是否有提供路邊停車， valet 是否有代客泊車。|
| payment_options ||
| phone |此地點的電話。|
| price_range |此地點的價格水準，`$, $$, $$$, $$$$` or Unspecified|
| rating_count |此地點的評分數量。|
| restaurant_services |此地點提供的服務。restaurant_services |
| restaurant_specialties |此地點提供的餐點。restaurant_specialties |
| single_line_address |此地點的地址以一行字串格式。|
| website |此地點的自有網站。|