# Model 服務佈署

- flask
- web server
- 套件

## Flask

開啟多執行序
`threaded=True`
`app.run(host="0.0.0.0", port=5000, debug=True, threaded=True)`

- 同時1000筆請求就有明顯的壅塞問題

## add Gunicorn

`pip install gunicorn`
