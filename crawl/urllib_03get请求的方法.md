>quote方法

```python

# https://www.baidu.com/s?wd=%E5%91%A8%E6%9D%B0%E4%BC%A6  
# 需求：获取https://www.baidu.com/s?wd=周杰伦的网页源码
  
import urllib.request  
import urllib.parse  
# url = "https://www.baidu.com/s?wd=周杰伦"#失败  
# url = "https://www.baidu.com/s?wd=%E5%91%A8%E6%9D%B0%E4%BC%A6"

url = "https://www.baidu.com/s?wd="  
  
# 请求对象定制为了解决反爬的第一种手段  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
  
# 将周杰伦三个字变成unicode编码的格式  
# 依赖于urllib.parse  
name = urllib.parse.quote("周杰伦")  
# print(name)  
# 此时的url拼接  
url = url + name  
# print(url)  
# 请求对象的定制  
request = urllib.request.Request(url=url,headers=headers)  
  
# 模拟浏览器向服务器发送请求  
response = urllib.request.urlopen(request)  
  
content = response.read().decode('utf-8')  
  
print(content)

```

>urlencode方法

```python

# urlencode应用于多个参数的场景  
import urllib.request  
import urllib.parse  
  
# # https://www.baidu.com/s?wd=周杰伦&sex=男  
#  
# data = {  
#     'wd':'周杰伦',  
#     'sex':'男',  
#     'location':'中国台湾省'  
# }  
#  
# a = urllib.parse.urlencode(data)  
# print(a)  
  
base_url = "https://www.baidu.com/s?"  
data = {  
    'wd':'周杰伦',  
    'sex':'男',  
    'location':'中国台湾省'  
}  
data = urllib.parse.urlencode(data)  
  
url = base_url + data  
  
# print(url)  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
  
request = urllib.request.Request(url=url,headers=headers)  
  
response = urllib.request.urlopen(request)  
  
content = response.read().decode('utf-8')  
  
print(content)

```