>get请求豆瓣电影前十页
>post请求KFC位置请求
>cookies

>get请求豆瓣电影前十页

```python

import urllib.request  
import urllib.parse  
import os  
  
# https://movie.douban.com/j/chart/top_list?type=5&interval_id=100%3A90&action=&  
# start=0&limit=20  
  
# https://movie.douban.com/j/chart/top_list?type=5&interval_id=100%3A90&action=&  
# start=20&limit=20  
  
# https://movie.douban.com/j/chart/top_list?type=5&interval_id=100%3A90&action=&  
# start=40&limit=20  
  
# page   1  2   3   4  
# start  0  20  40  60  
# start = (page - 1) * 20  
  
# get请求  
# 下载豆瓣电影前十页东西  
# （1）请求对象定制  
# （2）获取响应的数据  
# （3）下载数据  
  
def create_request(page):  
    base_url = 'https://movie.douban.com/j/chart/top_list?type=5&interval_id=100%3A90&action=&'  
  
    data = {  
        'start':(page - 1) * 20,  
        'limit':20  
    }  
    # get请求后不需要encode  
    data = urllib.parse.urlencode(data)  
    # print(data)  
    url = base_url + data  
    headers = {  
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
    }  
    # print(url)  
    request = urllib.request.Request(url=url,headers=headers)  
    return request  
  
def get_content(request):  
    response = urllib.request.urlopen(request)  
    content = response.read().decode('utf-8')  
    return content  
  
def down_load(page,content):  
  
    if not os.path.exists('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\doubanjson'):  
        os.mkdir('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\doubanjson')  
    with open('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\doubanjson\\' + 'douban' + str(page) + '.json', 'w', encoding='utf-8') as fp:  
        fp.write(content)  
    # 注意这个有点问题  
    # with open('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\doubanjson\\douban1-10.json', 'a', encoding='utf-8') as fp:  
    #     fp.write(content+'\n')if __name__ == '__main__':  
    start_page = int(input('请输入起始的页码：'))  
    end_page = int(input('请输入结束的页码：'))  
    for page in range(start_page,end_page + 1):  
        # 每一页都有自己请求对象的定制  
        request = create_request(page)  
        content = get_content(request)  
        down_load(page,content)

```

>post请求KFC位置请求

```python

# post请求  
  
# http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=cname  
  
# 每一页的formdata不一样，其中的pageIndex不一样  
# base_url = 'http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=cname'  
# data = {  
#     'cname': '北京',  
#     'pid':'',  
#     'pageIndex': '1',  
#     'pageSize': '10'  
# }  
import urllib.request  
import urllib.parse  
import os  
  
def create_request(page):  
    base_url = 'http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=cname'  
    headers = {  
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'  
    }  
    data = {  
        'cname': '北京',  
        'pid':'',  
        'pageIndex': str(page),  
        'pageSize': '10'  
    }  
    # post请求要进行编码  
    data = urllib.parse.urlencode(data).encode('utf-8')  
    request = urllib.request.Request(url=base_url,headers=headers,data=data)  
    return request  
  
def get_content(request):  
    response = urllib.request.urlopen(request)  
    content = response.read().decode('utf-8')  
    return content  
  
def down_load(page,content):  
    if not os.path.exists('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\KFCjson'):  
        os.mkdir('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\KFCjson')  
    with open('E:\\Pycharm\\workstation\\urlDemo\\chapter_02\\KFCjson\\' + 'KFC' + str(page) + '.json', 'w', encoding='utf-8') as fp:  
        fp.write(content)  
  
if __name__ == '__main__':  
    start_page = int(input('请输入起始的页码：'))  
    end_page = int(input('请输入结束的页码：'))  
    for page in range(start_page, end_page + 1):  
        request = create_request(page)  
        content = get_content(request)  
        down_load(page,content)

```

>cookies

```python

# 数据采集的时候，绕过登录，进入  
# 个人信息页面是utf-8，但是还报错了编码错误，因为没有进入到个人信息页面  
# 而是跳转到登录页面，登录页面不是utf-8  
# 请求头信息不够，所以访问不行  
import urllib.request  
import urllib.parse  
  
url = 'https://weibo.com/set/index'  
headers = {  
    'accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',  
    # 'accept-encoding': 'gzip, deflate, br',  
    'accept-language': 'zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6',  
    'cache-control': 'max-age=0',  
    # 'cookie': 'SINAGLOBAL=7348758555169.41.1658383387006; XSRF-TOKEN=nkJAiUOH4nLz-egZVnJFnj1z; PC_TOKEN=d44ba41dfc; login_sid_t=926fcb610e38eb3ace8ef3de40c809ee; cross_origin_proto=SSL; WBStorage=4d96c54e|undefined; _s_tentry=weibo.com; Apache=9378414069577.783.1659101965771; ULV=1659101965776:6:6:2:9378414069577.783.1659101965771:1658982540171; wb_view_log=1536*8641.25; ALF=1690638011; SSOLoginState=1659102013; SUB=_2A25P55NtDeRhGeNM7VEX-CzKzj-IHXVslIOlrDV8PUNbmtAfLRDjkW9NTh1gD4zQE_60qL4Tuo1eOColc4OA5Qs7; SUBP=0033WrSXqPxfM725Ws9jqgMF55529P9D9WhE3_.FHN21eBUg5zX-9ia15JpX5KzhUgL.Fo-ESoec1hzcSKe2dJLoI7yz9PLPIs8j9Btt; WBPSESS=TQo9Vp7zUR5KhFnxNhgy-lQvcWiP3jVdoss5vIHJR-q_zcPmZuzbZsxWORfPtTMXbtcDOJ4kLLppwmKA5Q8JSnDdeV7nLSTFmbHgpj49qBUa1euF9AXmSTzmFZd1hDWdqkQgEhWWKm1t6lR9a0Bn9A==',  
    'referer': 'https://login.sina.com.cn/',  
    'sec-ch-ua': '" Not;A Brand";v="99", "Microsoft Edge";v="103", "Chromium";v="103"',  
    'sec-ch-ua-mobile': '?0',  
    'sec-ch-ua-platform': '"Windows"',  
    'sec-fetch-dest': 'document',  
    'sec-fetch-mode': 'navigate',  
    'sec-fetch-site': 'same-origin',  
    'sec-fetch-user': '?1',  
    'upgrade-insecure-requests': '1',  
    'cookie': 'SINAGLOBAL=7348758555169.41.1658383387006; XSRF-TOKEN=nkJAiUOH4nLz-egZVnJFnj1z; PC_TOKEN=d44ba41dfc; login_sid_t=926fcb610e38eb3ace8ef3de40c809ee; cross_origin_proto=SSL; WBStorage=4d96c54e|undefined; _s_tentry=weibo.com; Apache=9378414069577.783.1659101965771; ULV=1659101965776:6:6:2:9378414069577.783.1659101965771:1658982540171; wb_view_log=1536*8641.25; ALF=1690638011; SSOLoginState=1659102013; SUB=_2A25P55NtDeRhGeNM7VEX-CzKzj-IHXVslIOlrDV8PUNbmtAfLRDjkW9NTh1gD4zQE_60qL4Tuo1eOColc4OA5Qs7; SUBP=0033WrSXqPxfM725Ws9jqgMF55529P9D9WhE3_.FHN21eBUg5zX-9ia15JpX5KzhUgL.Fo-ESoec1hzcSKe2dJLoI7yz9PLPIs8j9Btt; WBPSESS=TQo9Vp7zUR5KhFnxNhgy-lQvcWiP3jVdoss5vIHJR-q_zcPmZuzbZsxWORfPtTMXbtcDOJ4kLLppwmKA5Q8JSnDdeV7nLSTFmbHgpj49qBUa1euF9AXmSTzmFZd1hDWdqkQgEhWWKm1t6lR9a0Bn9A==',  
    'user-agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36 Edg/103.0.1264.71'  
}  
  
request = urllib.request.Request(url=url,headers=headers)  
  
response = urllib.request.urlopen(request)  
  
content = response.read().decode('utf-8')  
# content = response.read().decode('gb2312')  
# print(content)  
with open('weibo.html','w',encoding='utf-8') as fp:  
    fp.write(content)

```