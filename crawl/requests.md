1### get请求

```python

import requests  
url = "https://www.baidu.com/s"  
  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
data = {  
    'wd':'北京'  
}  
  
response = requests.get(url=url,params=data,headers=headers)  
  
content = response.text  
  
print(content)

```

### post请求

```python

import requests  
url = 'https://fanyi.baidu.com/sug'  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
  
data = {  
    'kw':'eye'  
}  
  
  
response = requests.post(url=url,data=data,headers=headers)  
response.encoding = 'utf-8'  
content = response.text  
  
import json  
  
obj = json.loads(content)  
  
print(obj)

```


### 实战

```python

# 通过登录，然后进入到主页面  
  
# 登录需要的参数  
# __VIEWSTATE: MGolCoEjIgnswMIJXPpMCNdyn3/1c2QlC4VxsHB7NTL4bVGZuXO8NXHXPw2NjcTtYRaKWsxelxSooXOsf4UJm+3jjEvaoqR1/UnZVqBvpOloYUwrOy6pPFg1GMMsv/NEpt073r1xbbh/qwlSAymhTuB3Uyo=  
# __VIEWSTATEGENERATOR: C93BE1AE  
# from: http://so.gushiwen.cn/user/collect.aspx  
# email: 1669903129@qq.com  
# pwd: 123456  
# code: GEPJ  
# denglu: 登录  
  
# 观察VIEWSTATE、VIEWSTATEGENERATOR和code  
# 页面源码中查看上述的变量，获取页面源码解析  
  
import requests  
  
# 登录页面的url地址  
url = 'https://so.gushiwen.cn/user/login.aspx?from=http://so.gushiwen.cn/user/collect.aspx'  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
# 获取页面源码  
response = requests.get(url=url,headers=headers)  
content = response.text  
  
# 解析页面源码  
from lxml import etree  
tree = etree.HTML(content)  
  
# 获取想要的数据  
VIEWSTATE = tree.xpath('//input[@id="__VIEWSTATE"]/@value')[0]  
VIEWSTATEGENERATOR = tree.xpath('//input[@id="__VIEWSTATEGENERATOR"]/@value')[0]  
  
# print(VIEWSTATE)  
# print(VIEWSTATEGENERATOR)  
  
# 获取验证码图片  
code = tree.xpath('//img[@id="imgCode"]/@src')[0]  
code_url = 'https://so.gushiwen.cn' + code  
# print(code_url)  
# 获取验证码的图片之后 下载到本地 观察验证码然后输入值  
  
# 错误  
# import urllib.request  
# urllib.request.urlretrieve(url=code_url,filename='code.jpg')  
# 使用requests内的session()，通过session返回值就可以  
session = requests.session()  
# 验证码的url内容  
response_code = session.get(code_url)  
# 注意此时要使用二进制数据，因为我们使用的是图片的下载  
content_code = response_code.content  
# wb的模式就是将二进制数据写入到文件  
with open('code.jpg','wb') as fp:  
    fp.write(content_code)  
  
code_name = input('请输入你的验证码：')  
  
#点击登录  
url_post = 'https://so.gushiwen.cn/user/login.aspx?from=http%3a%2f%2fso.gushiwen.cn%2fuser%2fcollect.aspx'  
  
data_post = {  
    '__VIEWSTATE': VIEWSTATE,  
    '__VIEWSTATEGENERATOR': VIEWSTATEGENERATOR,  
    'from': 'http://so.gushiwen.cn/user/collect.aspx',  
    'email': '1669903129@qq.com',  
    'pwd': 'JzY1027J',  
    'code': code_name,  
    'denglu': '登录'  
}  
  
response_post = session.post(url=url,headers=headers,data=data_post)  
content_post = response_post.text  
with open('gushiwen.html','w',encoding='utf-8') as fp:  
    fp.write(content_post)

```