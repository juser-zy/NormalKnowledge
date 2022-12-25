>基本使用

```python

from lxml import etree  
# xpath解析  
# （1）本地文件  
# （2）应用较多，服务器响应得数据 response.read().decode('utf-8')  
# 本地  
tree = etree.parse('xpath.html')  
  
# xpath基本语法  
# 1.路径查询：  
#     //:查找所有子孙节点，不考虑层级关系  
#      /:找直接子节点  
# 2.谓词查询：  
#     //div[@id]  
#     //div[@id='maincontent']  
# 3.属性查询：  
#     //@class  
# 4.模糊查询：  
#     //div[contains(@id,"he")]  
#     //div[starts-with(@id,"he")]  
# 5.内容查询：  
#     //div/h1/text()  
# 6.逻辑运算  
#     ///div[@id="head" and @class="s_down"]  
#     //title | //price  
  
# tree.xpath('xpath路径')  
  
# 查找ul下面得li  
# li_list = tree.xpath('//body//ul/li')  
# li_list = tree.xpath('//body//li')  
  
# 查找所有有id得属性得li  
# li_list = tree.xpath('//ul/li[@id]')  
# text()获取标签中得内容  
# li_list = tree.xpath('//ul/li[@id]/text()')  
  
# 找到id为l1的标签，注意引号  
# li_list = tree.xpath('//ul/li[@id="l1"]')  
  
# 查找到id为l2的li标签的class属性值  
# li_list = tree.xpath('//ul/li[@id="l2"]/@class')  
  
# 查找id内包含l的li标签  
# li_list = tree.xpath('//ul/li[contains(@id,"l")]/text()')  
# li_list = tree.xpath('//ul/li[starts-with(@id,"c")]/text()')  
  
# 查询id为l2，class为c1的数据  
# li_list = tree.xpath('//ul/li[@id = "l2" and @class="c1"]/text()')  
  
# li_list = tree.xpath('//ul/li[@id="l2" or @id = "c3"]/text()')  
  
li_list = tree.xpath('//ul/li[@id="l2"]/text() | //ul/li[@id="c3"]/text()')  
  
# 判断列表得长度  
print(li_list)  
print(len(li_list))

```

>解析百度一下

```python
  
# （1）获取百度网页的源码  
# （2）解析服务器响应的文件  
# （3）打印  
  
import urllib.request  
from lxml import etree  
url = 'https://www.baidu.com/'  
headers = {  
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
}  
request = urllib.request.Request(url=url,headers=headers)  
  
response = urllib.request.urlopen(request)  
  
content = response.read().decode('utf-8')  
  
# 解析网页源码，获取想要的数据  
# 解析服务器响应的文件  
tree = etree.HTML(content)  
  
# 获取想要的数据  
result = tree.xpath('//input[@id="su"]/@value')  
  
print(result)

```


>实战：站长素材前十页的图片

```python

# -*- coding: UTF-8 -*-  # @Time     :2022/7/30 17:05  
# @File     :xpath_03.py  
# @Author   :jzy  
# @Intro    :站长素材,前十页的图片  
  
import urllib.request  
import os  
from lxml import etree  
# https://sc.chinaz.com/tupian/index.html 1  
# https://sc.chinaz.com/tupian/index_2.html 2  
def create_request(page):  
    if page == 1:  
        url = 'https://sc.chinaz.com/tupian/index.html'  
    else:  
        url = 'https://sc.chinaz.com/tupian/index_' + str(page) + '.html'  
    # print(url)  
    headers = {  
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
    }  
    request = urllib.request.Request(url=url,headers=headers)  
    return request  
  
def get_content(request):  
    response = urllib.request.urlopen(request)  
    content = response.read().decode('utf-8')  
    return content  
  
def down_load(content):  
    if not os.path.exists('E:\\Pycharm\\workstation\\urlDemo\\chapter_03\\pic'):  
        os.mkdir('E:\\Pycharm\\workstation\\urlDemo\\chapter_03\\pic')  
#     下载图片  
# urllib.request.urlretrieve('图片地址','文件的名字')  
    tree = etree.HTML(content)  
    src_list = tree.xpath('//div[@id="container"]//a/img/@src')  
    name_list = tree.xpath('//div[@id="container"]//a/img/@alt')  
    print(len(src_list),len(name_list))  
    for i in range(len(name_list)):  
        name = name_list[i]  
        src = src_list[i]  
        # print(name,src)  
        url = 'https:' + src  
        urllib.request.urlretrieve(url=url,filename='./pic/'+name+'.jpg')  
  
if __name__ == '__main__':  
    start_page = int(input('起始页码'))  
    end_page = int(input('末尾页码'))  
    for page in range(start_page,end_page+1):  
        # （1）请求对象的定制  
        request = create_request(page)  
        # （2）获取网页源码  
        content = get_content(request)  
        # （2）下载  
        down_load(content)

```