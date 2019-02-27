# 오늘 배운 것

**https://ide.c9.io/mongan/python-telegram -나**

https://ide.c9.io/och8808/python-telegram- 선생님 



## 이메일 보내기

```python
import csv
import smtplib
from email.message import EmailMessage
from email.mime.appication import MIMEApplication

import getpass
password = getpass.getpass("password : ")

smtp = smtplib.SMTP_SSL("smtp.naver.com",465)
smtp.login("och8808", password)

#파일 찾기
filename = "giraffe.png"
with open(filename,"rb") as img_file:
    img = img_file.raed()

    with open("student_list.csv","r") as f:
        csv_reader = csv.reader(f)
    
        for student in csv_reader:
            msg = EmailMessage()
            msg["Subject"] = f"{student[0]} 삼성 공채 정보입니다."
            msg["From"] = "och8808@naver.com"
            msg["To"] = student[1]
            # msg.set_content(f"{student[0]}님이 오늘 {student[2]}하였습니다.")
            msg.add_alternative(
                '''
                <h1>삼성전자 서류 합격입니다.!</h1>
                <p>1차 면접 안내드립니다.</p>
                ''', subtype="html"
            )
            part = MIMEApplication(img, name=filename)
            msg.attach(part)
            
            smtp.send_message(msg)
            print(student[0])

        
        smtp.send_message(msg)
        print(student[0])

```



두번째 템플릿을 이용한 코드

```python
import csv
import smtplib
from email.message import EmailMessage
from email.mime.application import MIMEApplication
from jinja2 import Template
import os

import getpass
password = getpass.getpass("password : ")

smtp = smtplib.SMTP_SSL("smtp.naver.com",465)
smtp.login("qorauddks", password)

# filename = "1.jpg"
# with open(filename,"rb") as img_file:
#     img = img_file.read()


        

with open("email.html","r", encoding="utf-8") as html:
    email_tamplate = html.read()

t = Template(email_tamplate)

with open("student_list.csv","r") as f:
    csv_reader = csv.reader(f)    
    
    for student in csv_reader:
        msg = EmailMessage()
        msg["Subject"] = f"{student[0]} 삼성 공채 정보입니다."
        msg["From"] = "och8808@naver.com"
        msg["To"] = student[1]
        # msg.set_content(f"{student[0]}님이 오늘 {student[2]}하였습니다.")
        render_html = t.render(name=student[0])
        for filename in os.listdir("./images"):
            with open(f'./images/{filename}',"rb") as f:
                img = f.read()
                
                part = MIMEApplication(img, name=filename)
                part.add_header("Content-ID",f'<{filename}>')
                msg.attach(part)
        msg.add_alternative(render_html, subtype="html")
        # part = MIMEApplication(img, name=filename)
        # msg.attach(part)
        
        smtp.send_message(msg)
        print(student[0])

```

html 파일안에서 이미지를 변경하려면 cid:를 붙여줘야 한다.





### html email template

무료사이트 http://mailbakery.com





### 플라스크 이용 웹서버 만들기

```python
#플라스크를 사용할거야 라고 알려주는 문장
from flask import Flask
app = Flask(__name__)


#
@app.route("/")
def hello():
    return "Hello World!"


#서버실행 옵션    
if __name__ == '__main__':
    app.run(debug=True,host="0.0.0.0",port=8080) #app.py flask run이랑 같은 역할
#python app.py가 작동할 수 있는 이유는 조건문을 적어놨기 때문

```

$ pip install Flask
$ FLASK_APP=hello.py flask run #명령창에 입력



- 라우팅 

  @app.route

  함수():

  ​	return ""

- ! 하고 tab을 누르면  자동완성

- lorem picsum 더미사진가져오는 곳
- 2019 웹개발자라고 검색하면 웹개발자가 필요한 역량이 나옴



tip

- seo검색엔진최적화 : h1태그는 가장 중요한 내용만 있어야 한다
- flexbox froggy :  css flex 속성 공부하는 사이트





#### giphy api

```python
@app.route("/ego/<string:name>")
def ego(name):
    url = "http://api.giphy.com/v1/gifs/search?api_key=9QbiAyTBdiZkedo35sYeeuMft6z8oN2D&q="
    fake = Faker("ko_KR")
    job = fake.job()
    return render_template("ego.html", name=name,job=job)
```

jason viewer chrome.





## GIT이란?!

git은 터미널에서 명령어만으로 작업할 수 있다(x) -->sourcetree,github를 이용하면 편리함

작업한 파일 ---add--> 커밋할 목록 ----commit--->쌓인커밋들-----push---> github

##### git add . 

 : 모든 파일 커밋하는 명령

##### git commit



1. git으로 관리할거야 ---> git init :로컬 저장소 만들기
2. git add , 



collaborators에서 같이 수정할 수 있는 권한이 생기는 것이다.



- clone vs pull

새롭게 복사하거나 vs  받아온걸 업데이트하거나

pull을 이용해서 받아올 수 있따.

- git status : gitt add한것을 알려주는 명령어
- git log --graph

! vim adventures  : vim에 익숙해지는 사이트