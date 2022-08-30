```python

# UA:User Agent  
# 语法：request = urllib.request.Request()
import urllib.request  
url = 'https://www.baidu.com'  
  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
  
# 因为urlopen方法中不能存储字典，所以headers无法传递过去  
# 请求对象的定制  
# 注意因为参数顺序的问题，不能直接写url和headers  
request = urllib.request.Request(url=url,headers=headers)  
response = urllib.request.urlopen(request)  
content = response.read().decode('utf-8')  
  
print(content)

```