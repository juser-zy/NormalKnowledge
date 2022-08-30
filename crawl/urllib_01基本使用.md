```python

import urllib.request  
  
# （1）定义一个url  
url = "http://www.baidu.com"  
  
# （2）模拟浏览器向服务器发送请求  
response = urllib.request.urlopen(url)  
  
# （3）获取响应中的页面源码  
# read 返回的是字节形式的二进制数据  
# 将二进制数据转换为字符串，即解码  
content = response.read().decode("utf-8")  
  
print(content)

```
