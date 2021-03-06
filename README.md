# 如何使用LINE 打造屬於自己的對話機器人

## Target  
LINE為目前台灣主流的通訊軟體，並且提供相對應的Message API可供人員直接開發聊天機器人，本章節將會教您手把手的設計一套聊天機器人。當您熟悉了LINE的API之後以及Pytthon程式碼之後，您便可以開始逐步打造一台擁有自身功能的機器人了。

## Requirement  

 - 略懂Python語法
 -  擁有LINE的帳號 
 - 使用過Flask的Python Micro Framwork ... 更好
 - Heroku帳號(需要把軟體部署到PAAS的環境，以便提供長期服務，更重要的是它已經提供Https的網路協定了)

## Tools  

 - IDE(任何一種都可以，推薦Sublime/Atom/**Visul Studio Code**) 
 - 作業系統(皆可! 推薦使用Linux or MacOS) 
 - GIT(版本控制工具) 
 - Heroku-CLI (部署工具)

## Start Up  

 1. 申請LINE Developer帳號 
 2. 申請Heroku的帳號 
 3. 安裝Heroku-CLI
 4. 安裝GIT(使用Linux-Ubuntu可以省略) 
 5. 安裝Python3.6 with PIP 
 6. 使用PIP 安裝 Flask、LinebotAPI 
 7. 複製Sample Code(App.py) 
 8. 上傳程式碼至Heroku 完成


## 先Line@官方網站註冊申請你的LINE@帳號  
需要接著你需要再設定取得你機器人的`CHANNEL_SECRET` 還有 `ACCESS_TOKEN`(這兩個務必保管好，他是你機器人工作的主要工具)
[LINE官方網站](http://at.line.me/tw/entry)

## 前往Heroku並且註冊一個帳號，然後在電腦上安裝Heroku-CLI  
當註冊完畢並且Heroku-CLI也安裝完畢的時候，您就是可在`終端機`的頁面輸入`Heroku login`，然後輸入你的帳號跟密碼就可以開始與Heroku的Server連動囉。接著我們要創建一個資料夾來放置我們的專案。  

## Heroku-CLI安裝
```shell
sudo apt-get install software-properties-common # debian only
sudo add-apt-repository "deb https://cli-assets.heroku.com/branches/stable/apt ./"
curl -L https://cli-assets.heroku.com/apt/release.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install heroku
```

## 安裝PIP(套件管理程式)
```
sudo apt install python-pip python-dev python-essential
```

## 安裝LINE bot SDK
LINE已經為我們準備好了好幾隻API囉!所以我們要使用的功能很簡單，不需要重新建造輪胎了
``` bash
# install line bot sdk
pip install line-bot-sdk
```

## 安裝網路服務微框架  Flask
```bash
pip install flask
sudo pip install --upgrade pip 
sudo pip install --upgrade virtualenv 
```

## 創建一個LINE聊天機器人的專案
```bash
mkdir python_line_robot ## 創建資料夾
cd python_line_robot ## 進入我們的資料夾
touch app.py ## 創建一個名為app的檔案
```


## 撰寫requirements.txt 來讓Heroku，我們的微型服務目前還需要哪寫Python套件
撰寫`requirements.txt`的目地在於讓我們把project上傳到Heroku的時候，Heroku會幫我們去找到相依性的套件，就不用一個一個傻瓜安裝了。  
```bash
pip freeze > requirements.txt
```

## 添加程式碼至app.py
**app.py**
``` python
from flask import Flask, request, abort

from linebot import (
    LineBotApi, WebhookHandler
)
from linebot.exceptions import (
    InvalidSignatureError
)
from linebot.models import (
    MessageEvent, TextMessage, TextSendMessage,
)

app = Flask(__name__)

line_bot_api = LineBotApi('YOUR_CHANNEL_ACCESS_TOKEN') ## 填入你的AccessToken
handler = WebhookHandler('YOUR_CHANNEL_SECRET') ## 填入你的Channel Secret

## 系統會掛在callback的路徑之上，但傳送訊息給機器人之後，LINE會主動進行Callback的動作通知你的Server並且把資訊都提交給你，之後你便可以將使用者的訊息內容，修改或者萃取重點，並且回傳給USER
@app.route("/callback", methods=['POST'])
def callback():
    # get X-Line-Signature header value
    signature = request.headers['X-Line-Signature']

    # get request body as text
    body = request.get_data(as_text=True)
    app.logger.info("Request body: " + body)

    # handle webhook body
    try:
        handler.handle(body, signature)
    except InvalidSignatureError:
        abort(400)

    return 'OK'


@handler.add(MessageEvent, message=TextMessage)
def handle_message(event):
    line_bot_api.reply_message(
        event.reply_token,
        TextSendMessage(text=event.message.text))


if __name__ == "__main__":
    app.run()

```

## 部署程式碼至Heroku
> 記得要先至Heroku註冊一個免費帳號使用  
```bash
heroku login # 登入Heroku 系統會要求你輸入帳號跟密碼
git add * 
git commit -am "start to run yout bot"
git push heroku master ##上傳檔案至Heroku部署你的程式碼
```

## 設定LineBot的Callback網址  
回到LINE機器人的設定網頁，讓我們將Heroku提供的網址回填至此，並且測試連線是否正確。

![img](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)