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
import random

#보안상의 이유로 환경변수 설정
token= os.getenv("TELE_TOKEN")
naver_id = os.getenv("NAVER_ID")
naver_secret = os.getenv("NAVER_SECRET")

api_url = "https://api.hphk.io/telegram"
#https://api.telegram.org 

@app.route("/")
def hello():
    return "Hello World!"

@app.route(f"/{token}", methods=["POST"])
def telegram():
    msg_info = (request.get_json())
    #메세지를 보낸 사람의 아이디
    chat_id = msg_info.get("message").get("from").get("id") #msg_info["id"], 대괄호로 가져오면 오류가 날 수 있는데 get으로 가져오면 데이터가 없어도 null로 가져온다.
    #사용자가 보낸 메세지
    text= msg_info.get("message").get("text")
    return_text = "임시"
    
    if msg_info.get("message").get("photo") is not None:
        #사진이 있을 때
        #telegram에서 이미지 가져오기
        file_id = msg_info.get("message").get("photo")[-1].get("file_id")
        file_res = requests.get(f"{api_url}/bot{token}/getFile?file_id={file_id}")
        file_path = file_res.json().get("result").get("file_path")
        file_url = f"{api_url}/file/bot{token}/{file_path}"
        #return_text = file_url
        
        real_file = requests.get(file_url,stream=True)
        headers = {
                "X-Naver-Client-Id":naver_id,
                "X-Naver-Client-Secret":naver_secret
            }
        naver_url ="https://openapi.naver.com/v1/vision/celebrity"
        clova = requests.post(naver_url,headers=headers,files={"image":real_file.raw.read()})
        
        if clova.json().get("info").get("faceCount"):
            return_text = clova.json().get("faces")[0].get("celebrity")
            #사람이 인식 될때
            pass
        else:
            #사람이 인식되지 않을 때
            return_text = "사람이 없어요"
        
    else:
        #사진이 없을 때
    
        if text == "로또":
            return_text = random.sample(range(1,46),6)
        elif text == "메뉴":
            menu_list = ["양자강","명동칼국수","김밥카페","시골집"]
            return_text = random.choice(menu_list)
        elif text[0:3] == "번역 ":
            headers = {
                "X-Naver-Client-Id":naver_id,
                "X-Naver-Client-Secret":naver_secret
            }
            naver_url ="https://openapi.naver.com/v1/papago/n2mt" #이 url로 데이터 넣을게
            data = {
                "source":"ko",
                "target":"en",
                "text": text[3:]
            }
            #요청하기
            papago = requests.post(naver_url,headers=headers,data=data)
            return_text = papago.json().get("message").get("result").get("translatedText")
        else:
            return_text = "없는 명령어입니다.현재 제공되는 명령어는 로또와 메뉴입니다."
    
    
    
    return_url = f"{api_url}/bot{token}/sendMessage?chat_id={chat_id}&text={return_text}"
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



#### 네이버개발자 센터

애플리케이션 정보

Client ID Client Secret 을 발급받을 수 있다. 환경변수에 추가하기

c9 ~/.bashrc --->c9에서만 열리는 코드 마지막

```python
export NAVER_ID="토큰"
export NAVEER_SECRET=""
```

app.py에 추가(환경변수 설정)

```python
naver_id = os.getenv("NAVER_ID")
naver_secret = os.getenv("NAVER_SECRET")
```

###### 번역기능 추가

```python
@app.route(f"/{token}", methods=["POST"])
def telegram():
    msg_info = (request.get_json())
    #메세지를 보낸 사람의 아이디
    chat_id = msg_info.get("message").get("from").get("id") #msg_info["id"], 대괄호로 가져오면 오류가 날 수 있는데 get으로 가져오면 데이터가 없어도 null로 가져온다.
    #사용자가 보낸 메세지
    text= msg_info.get("message").get("text")
    
    if text == "로또":
        return_text = random.sample(range(1,46),6)
    elif text == "메뉴":
        menu_list = ["양자강","명동칼국수","김밥카페","시골집"]
        return_text = random.choice(menu_list)
    elif text[0:3] == "번역 ":
        headers = {
            "X-Naver-Client-Id":naver_id,
            "X-Naver-Client-Secret":naver_secret
        }
        naver_url ="https://openapi.naver.com/v1/papago/n2mt" #이 url로 데이터 넣을게
        data = {
            "source":"ko",
            "target":"en",
            "text": text[3:]
        }
        #요청하기
        papago = requests.post(naver_url,headers=headers,data=data)
        return_text = papago.json().get("message").get("result").get("translatedText")
    else:
        return_text = "없는 명령어입니다.현재 제공되는 명령어는 로또와 메뉴입니다."
    
```

###### 사진인식 기능 추가(이미지 분석 api사용)

```python
if msg_info.get("message").get("photo") is not None:
        #사진이 있을 때
        #telegram에서 이미지 가져오기
        file_id = msg_info.get("message").get("photo")[-1].get("file_id")
        file_res = requests.get(f"{api_url}/bot{token}/getFile?file_id={file_id}")
        file_path = file_res.json().get("result").get("file_path")
        file_url = f"{api_url}/file/bot{token}/{file_path}"
        #return_text = file_url
        
        real_file = requests.get(file_url,stream=True)
        headers = {
                "X-Naver-Client-Id":naver_id,
                "X-Naver-Client-Secret":naver_secret
            }
        naver_url ="https://openapi.naver.com/v1/vision/celebrity"
        clova = requests.post(naver_url,headers=headers,files={"image":real_file.raw.read()})
        
        if clova.json().get("info").get("faceCount"):
            return_text = clova.json().get("faces")[0].get("celebrity")
            #사람이 인식 될때
            pass
        else:
            #사람이 인식되지 않을 때
            return_text = "사람이 없어요"
```

사진 정확도 추가

```python

```

