>post请求百度翻译，请求的参数注意需要编码
>详细请求的时候需要注意headers和data

```python

import urllib.request  
import urllib.parse  
import json  
  
# url = 'https://fanyi.baidu.com/v2transapi?from=en&to=zh'  
# data = {  
#     'query':'spider'  
# }  
url = 'https://fanyi.baidu.com/sug'  
data = {  
    'kw':'spider'  
}  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
  
  
# post请求的参数必须要进行编码  
data = urllib.parse.urlencode(data).encode('utf-8')  
# post请求的参数不会拼接在url后面，而是放在请求的对象的data参数  
request = urllib.request.Request(url=url,data=data,headers=headers)  
# 模拟浏览器向服务器发送请求  
response = urllib.request.urlopen(request)  
  
content = response.read().decode('utf-8')  
  
obj = json.loads(content)  
  
print(obj)

```

>详细请求

```python

import urllib.request  
import urllib.parse  
url = 'https://fanyi.baidu.com/v2transapi?from=en&to=zh'  
  
headers = {  
    # 'Accept': '*/*',  
    # # 'Accept-Encoding': 'gzip, deflate, br',    # 'Accept-Language': 'zh-CN,zh;q=0.9',    # 'Acs-Token': '1658991609277_1658991728721_fsWQ2EFSY5EudlByr1bbmtl1Us7FDEtsRoahOjCA5ynG81+pjF77fToupV8ALD7lhn7yP5rIgrlx5hfiEqxNBWPYsT/WSdnRnVmLnybozKe8FWp1sYgIRToGdQXPPwMAjh+IxrnJZTxkdWr9NG5l5uNDt97oXj87B4VLW5EWDbKOGdI/gTsUQFLy5hkj2wcQH364zkeEVjb+ulO9G9gtI62ey4pe15J1kg/EZpwU3Nhc8w3zSKw8bxsCW97qFpDG5csPMWE4bwFwSXMG8zYz015NrRBKReg+79joshZzV9M=',    # 'Connection': 'keep-alive',    # 'Content-Length': '135',    # 'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',    'Cookie': 'BIDUPSID=6BC62023FEBA1EABABC1116872DE806B; PSTM=1658453560; BDUSS=dwZlVmbGQyVEdPQ1JqcWlIZmx3aUowT3h-bjhNS1ktaHRuS2VES2UyZUJGUWRqSVFBQUFBJCQAAAAAAAAAAAEAAACgBksuYTE2Njk5MDMxMjkAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIGI32KBiN9iaD; BDUSS_BFESS=dwZlVmbGQyVEdPQ1JqcWlIZmx3aUowT3h-bjhNS1ktaHRuS2VES2UyZUJGUWRqSVFBQUFBJCQAAAAAAAAAAAEAAACgBksuYTE2Njk5MDMxMjkAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIGI32KBiN9iaD; BAIDUID=1CA9CB225294DE49F1E21598312FCDEA:SL=0:NR=10:FG=1; newlogin=1; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; APPGUIDE_10_0_2=1; FANYI_WORD_SWITCH=1; REALTIME_TRANS_SWITCH=1; HISTORY_SWITCH=1; SOUND_PREFER_SWITCH=1; SOUND_SPD_SWITCH=1; BA_HECTOR=aha08l818l8h2h0la0266ckk1he20t217; ZFY=uywGGgNBw5om4kIkP4yVT27q:A1FuqnUo6KI0Iy:ADfnI:C; ariaDefaultTheme=undefined; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; delPer=0; PSINO=7; H_PS_PSSID=36548_36466_36725_36414_36664_36954_36948_36364_36802_36885_36789_36743_26350_36863_36937; Hm_lvt_64ecd82404c51e03dc91cb9e8c025574=1658848716,1658991651; Hm_lpvt_64ecd82404c51e03dc91cb9e8c025574=1658991685; ab_sr=1.0.1_YjAyYTJlODI5MjE0M2NmZWUxNjNmMzBjMmI5MDkyYmYyYWYwYTZmMDk0OWUzYzU1YzkxOGE3MThjYTY4NjFiZDc2ZjYyZTMzZDhkZTY5NDIxZmJlZGMyOTc0Yzk4YzIzMjhiYjQzZmQyMTE3ZmQwNzFiY2M3NDU4NjgxYzM4MTE4MmVhYzU1ZTgyMTM0MjQ1MzQ2MmRjNDQ2YTkwZjQyODIzYTgyMDYyY2IxNmI1YjUzY2M1NWNkMGU0Yzk3YTE2',  
    # 'Host': 'fanyi.baidu.com',  
    # 'Origin': 'https://fanyi.baidu.com',    # 'Referer': 'https://fanyi.baidu.com/',    # 'sec-ch-ua': '".Not/A)Brand";v="99", "Google Chrome";v="103", "Chromium";v="103"',    # 'sec-ch-ua-mobile': '?0',    # 'sec-ch-ua-platform': '"Windows"',    # 'Sec-Fetch-Dest': 'empty',    # 'Sec-Fetch-Mode': 'cors',    # 'Sec-Fetch-Site': 'same-origin',    # 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36',    # 'X-Requested-With': 'XMLHttpRequest'}  
  
data = {  
    'from': 'en',  
    'to': 'zh',  
    'query': 'love',  
    'transtype': 'realtime',  
    'simple_means_flag': '3',  
    'sign': '198772.518981',  
    'token': 'f99a75a3369de6bdd589e75286cc9cfb',  
    'domain': 'common'  
}  
# post 请求的参数必须进行编码，调用encode方法  
data = urllib.parse.urlencode(data).encode('utf-8')  
# 请求对象的定制  
requests = urllib.request.Request(url=url,data=data,headers=headers)  
# 模拟浏览器向服务器发送请求  
response = urllib.request.urlopen(requests)  
  
content = response.read().decode('utf-8')  
  
import json  
obj = json.loads(content)  
print(obj)

```