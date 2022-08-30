>**一般默认情况对象无法写入到文件中，需要采用序列化操作**

```python
fp = open('text.txt','w')  
name_list = ['zhangsan','lisi']  
# 默认情况对象无法写入到文件中，可以采用序列化操作  
fp.write(name_list)  
fp.close()
#报错：TypeError: write() argument must be str, not list
```

>**序列化操作，两种方式**
>dumps()
>dump()

#### **dumps()**
```python
# dumps(),应用于scrapy框架  
# (1)创建一个文件  
fp = open('text.txt','w')  
# （2）定义一个列表  
name_list = ['zs','ls']  
# （3）导入json模块  
import json  
# （4）序列化，将python对象变成json字符串  
names = json.dumps(name_list)  
# （5）写入并关闭  
fp.write(names)  
fp.close()
```
#### **dump()**
```python
# 在将对象转换为字符串的同时，指定一个文件的对象，然后把转换后的字符串写入到该文件中
# 修改一行代码
json.dump(name_list,fp)
```

---
>反序列化操作

#### **loads**
```python
# loads  
fp = open('text.txt','r')  
content = fp.read()  
print(content)  
results = json.loads(content)  
print(type(results))  
fp.close()
```
#### **load**
```python
# load  
fp = open('text.txt','r')  
result = json.load(fp)  
print(result)  
print(type(result))  
fp.close()
```

| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单 | 单元格 | 单元格 |
| 单元格 | 单 | 单元格 |





