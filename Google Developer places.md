# Google Developer places
## findplacefromtext
最一般的搜尋，用地址或電話搜尋。

HTTP URL：

	https://maps.googleapis.com/maps/api/place/findplacefromtext/output?parameters
	
`output` 可以換成 `json`、`xml` 來選擇輸出格式。

`parameters` 按照URL中的標準，所有參數都使用＆字符分隔`&`。

### **必要參數**
-	`key` : 你應用程式的API KEY。
-	`input` : 你想要搜尋的地址或電話號碼。字串，輸入非字串會錯誤。
- 	`inputtype` : 輸入 `input` 的類型可以是 `textquery`、`phonenumber `。 電話號碼必須為國際格式（以加號（“ +”）開頭，然後是國家/地區代碼，然後是電話號碼本身）

### **可選參數**
-	`language` : 語言代碼，指示應以哪種語言返回結果。搜索也會偏向所選語言；所選語言的搜索結果可能會獲得更高的排名。
- 	`fields` : 要回傳的資料，以逗號分開 `,` 。
- 	`locationbias` : 搜尋的地理範圍。如果未指定此參數，則默認情況下，API使用IP地址。
	-  	IP : 使用API的IP位置。傳遞字符串`ipbias`。
	-   	點 : 一個緯度/經度坐標。使用以下格式：`point:lat,lng`。
	-    圓形 : 以米為單位指定半徑的字符串，以十進制度表示的經度/緯度。使用以下格式：circle:radius@lat,lng。