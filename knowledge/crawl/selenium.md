### 基本使用

```python

from selenium import webdriver  
  
# 创建浏览器操作对象  
path = 'chromedriver.exe'  
browser = webdriver.Chrome(path)  
  
# 访问网站  
# url = 'https://www.baidu.com'  
url = 'https://www.jd.com/'  
browser.get(url)  
# 获取网页源码  
content = browser.page_source  
print(content)

```

### 元素定位

```python

from selenium import webdriver  
  
path = 'chromedriver.exe'  
browser = webdriver.Chrome(path)  
  
url = 'https://www.baidu.com'  
browser.get(url)  
  
# 元素定位  
# button = browser.find_element(by='id',value="su")  
# print(button)  
  
# button = browser.find_element(by='name',value="wd")  
# print(button)  
  
button = browser.find_element(by='xpath',value='//input[@id="su"]')  
print(button)

```

### 元素信息和交互

```python

from selenium import webdriver  
from selenium.webdriver.common.by import By  
  
path = 'chromedriver.exe'  
browser = webdriver.Chrome(path)  
url = 'https://www.baidu.com'  
browser.get(url)  
  
input = browser.find_element(by=By.ID,value='su')  
  
print(input.get_attribute('class'))  
print(input.get_attribute('value'))  
  
print(input.tag_name)  
  
a = browser.find_element(By.LINK_TEXT,"新闻")  
print(a.text)

```


### 交互

```python

from selenium import webdriver  
from selenium.webdriver.common.by import By  
  
path = 'chromedriver.exe'  
browser = webdriver.Chrome(path)  
url = 'https://www.baidu.com'  
browser.get(url)  
  
import time  
time.sleep(2)  
  
# 获取文本框的对象  
input = browser.find_element(By.ID,'kw')  
  
# 在文本框中输入周杰伦  
input.send_keys('周杰伦')  
time.sleep(2)  
  
button = browser.find_element(By.ID,'su')  
button.click()  
  
time.sleep(2)  
# 划到底部  
js_bottom = 'document.documentElement.scrollTop=100000'  
browser.execute_script(js_bottom)  
  
time.sleep(2)  
  
# 获取下一页的按钮  
next = browser.find_element(By.XPATH,'//a[@class="n"]')  
next.click()  
time.sleep(2)  
  
# 返回上一页  
browser.back()  
  
time.sleep(2)  
  
browser.quit()

```