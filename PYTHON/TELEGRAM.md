## TELEGRAM

#### TELEGRAM 사용하기

botfather 검색 후 newbot하기->토큰값 받아오기

#### 	message.py

```python
import requests
from bs4 import BeautifulSoup
import os
token = os.getenv("TELE_TOKEN") #token값을 올리면 악용할 수 있다.->환경변수에 토큰 저장
chat_id = "687830439"
# url = f"https://api.telegram.org/bot{token}/sendMessage?chat_id={chat_id}&text=안녕하세요"
url = f"https://api.hphk.io/telegram/bot{token}/sendMessage?chat_id={chat_id}&text="

sise_url = "https://finance.naver.com/sise/"
sise_html = requests.get(sise_url).text
sise_soup = BeautifulSoup(sise_html, "html.parser")
sise = sise_soup.select_one("#KOSPI_now").text
print(url)
res = requests.get(url+sise)
print(res)
# print(url)
```



#### app.py

```python
from flask import Flask,request
app = Flask(__name__)

import os
import requests

token= os.getenv("TELE_TOKEN")
api_url = "https://api.hphk.io/telegram"
#https://api.telegram.org 

@app.route("/")
def hello():
    return "Hello World!"

@app.route(f"/{token}", methods=["POST"])
def telegram():
    msg_info = (request.get_json())
    chat_id = msg_info.get("message").get("from").get("id") 
    #msg_info["id"], 대괄호로 가져오면 오류가 날 수 있는데 get으로 가져오면 데이터가 없어도 null로 가져온다.
    text= msg_info.get("message").get("text")
    
    return_url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={text}"
    requests.get(return_url)
    
    return '',200
    

if __name__=="__main__":
    app.run(debug=True, host="0.0.0.0",port=8080)
```



1. 토큰값은 환경변수에 저장해서 도용할 수 없게 하기
   - vi ~/.bashrc 로 들어가서 마지막에 export TELE_TOKEN="토큰번호"
   - exec $SHELL --> python message.py입력



https://api.telegram.org/bot토큰?url=https://python-telegram-mongan.c9users.io/토큰

url에 입력하면 나오는 페이지

```python
{
  "ok": true,
  "result": true,
  "description": "Webhook was set"
}
```



