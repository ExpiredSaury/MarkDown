## (一)网路爬虫入门

#### 1.0 爬虫是个啥

通过编写程序，模拟浏览器去上网，然后让其去互联网上抓取数据的过程

#### 1.1 爬虫分类

* 通用爬虫 ：抓取系统重要组成部分，抓取一整张页面的数据
* 聚焦爬虫：建立在通用爬虫基础之上，抓取的是页面中特定的局部内容
* 增量式爬虫 ：检测网站中数据更新的情况，只会抓取网站中最新更新出来的数据

#### 1.2 爬虫的矛与盾：

* 反爬机制： 门户网站，可以通过制定相应的策略或者技术手段，防止爬虫程序进行网站数据的爬取
* 反反爬策略： 爬虫程序可以通过指定相关策略或者技术手段，破解门户网站中具备的反爬机制，从而可以获取门户网站中相关的数据

#### 1.3 robots.txt协议：

* 君子协议，规定了网站中哪些数据可以被爬虫爬取，哪些数据不可以被爬虫爬取

  ```bash
  #查看淘宝网站的robots.txt协议
  www.taobao.com/robots.txt
  ```

#### 1.4 HTTP&HTTPS协议

* 当前URL地址在数据传输的时候遵循的HTTP协议
* 协议：就是两个计算机之间为了能够流畅的进行沟通而设置的一个君子协议，常见的协议有TCP/IP SOAP协议，HTTP协议， SMTP协议等...
* **HTTP协议**： Hyper Text Transfer Protocol(超文本传输协议) 的缩写，是用于万维网服务器传输超文本到浏览器的传送协议
* **HTTPS协议**：安全的超文本传输协议
* 直白点，**就是浏览器和服务器之间的数据交互遵守的HTTP协议**
* HTTP协议把一条消息分为三大快内容，无论是请求还是响应的三块内容

```python
# 加密方式：
# 对称密钥加密，
# 非对称密钥加密，
# 证书密钥加密
```

```python
# 请求：
'''
1.请求行--》请求方法（get/post） 请求url地址，协议
2.请求头--》放一些服务器要使用的附加信息
3
4.请求体--》一般放一些参数
'''
```

```python
#请求头中常见的一些重要内容（爬虫需要）
'''
1.USer-Agent：请求载体的身份标识（用啥发送的请求）
2.Refer：防盗链（这次请求是从哪些页面来的？ 反爬会用到）
3.cookie:本地字符串数据信息（用户登录信息，反爬的token）
Connection:请求完毕后，是保持连接还是断开连接
'''
```

```python
#响应：
'''
1.状态行--》协议 状态码 (200 404 500)
2.响应头--》放一些客户端要使用的一些附加信息
3.
4.响应体--》服务器返回的真正客户端要使用的内容（HTML，json）等
'''
```

```python
#响应头中一些重要内容
'''
Content-Type:服务器响应回客户端的数据类型
1.cookie:本地字符串数据信息（用户登录信息，反爬的token）
2.各种神奇的莫名其妙的字符串（这个需要经验了，一般都是token字样，防止各种攻击和反爬）
'''
```

```python
#请求方式：
''''
GET：显示提交
POST：隐示提交
'''
```

## (二)requests模块

作用：模拟浏览器 发起请求

#### 2.0 第一个爬虫程序

需求：爬取搜狗首页页面数据

```python
导包
import requsets
# 指定url
url='https://www.sogou.com/'
#发起请求
#get方法会返回一个响应对象
response=requests.get(url=url)
#获取响应数据：调用响应对象的text属性，  返回响应对象中存储的是字符串形式的响应数据（页面源码数据）
page_text=response.text
print(page_text)
#持久化存储
with open("sougou.html",'w',encoding='utf-8') as f:   		
    f.write(page_text)
print('爬取结束')
```

#### 2.1 requests模块实战

* text 返回字符串
* content 返回二进制
* json() 返回对象

**案例一**：爬取搜狗指定词条对应的结果（简易网页采集器）

```python
import requests
get_url='https://www.sogou.com/web?'
#处理url携带的参数，存到字典中
kw=input("enter a message:")
param={
    "query":kw
}
#UA伪装 ：让爬虫对应的请求载体身份标识 伪装成某一款浏览器
#UA：User-Agent  门户网站的服务器会检测对应请求的载体身份标识，如果检测到请求载体的身份标识是一款浏览器，，说明是一个正常的请求，但是检测到请求载体不是某一款浏览器，则表示该请求不正常（crawl），服务器端拒绝请求
headers={
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36"
}
response=requests.get(url=get_url,params=param,headers=headers)
page_text=response.text
# print(page_text)
FielName=kw+'.html'
with open(FielName,'w',encoding='utf-8') as f:
    f.write(page_text)
print(kw +' save  over!!')
```

**案例二**：百度翻译

```python
#拿到当前单词所对应的翻译结果
#页面局部刷新 ajax
import json
import requests
post_url='https://fanyi.baidu.com/sug'

headers={
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36"
}
word=input('请输入要翻译的单词：enter a word!!\n')
#post请求参数处理
data={
    "kw":word
}
response=requests.post(url=post_url,data=data,headers=headers)
# print(response.text)
dic_obj=response.json()#json返回的是字典对象，确认响应数据是json类型，才可使用json()
with open(word+'.json','w',encoding='utf-8') as f:
    json.dump(dic_obj,fp=f,ensure_ascii=False)
print('over')
```

**案例三**：豆瓣电影

```python
#页面局部刷新，发起ajax请求
import json
import requests
get_url='https://movie.douban.com/j/chart/top_list'
headers={
"User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36"
}
param={
    'type':'24',
    'interval_id' : '100:90',
    'action': '',
    'start': '0',#从库中第几部电影去爬取
    'limit': '20', #一次取出多少个
}

respons=requests.get(url=get_url,params=param,headers=headers)
list_data=respons.json()
fp=open('./douban.json','w',encoding='utf-8')
json.dump(list_data,fp,ensure_ascii=False)
fp.close()
print('over')
```

**案例四**：肯德基官网餐厅查询,爬取多页餐厅信息

```python
动态加载，局部刷新，
import json
import requests

post_url = 'http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=keyword'
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36"
}

for i in range(1,11):
    data = {
        'cname': '',
        'pid': '',
        'keyword': '北京',
        'pageIndex': i,
        'pageSize': '10',
    }

    response requests.post(url=post_url,headers=headers, data=data)
    dic_data = response.text
    with open('KFC_order', 'a', encoding='utf-8') as f:
        f.write(dic_data)
        print('\n')
```

```python
#爬取肯德基餐厅第一页数据
import requests,os

post_url='http://www.kfc.com.cn/kfccda/ashx/GetStoreList.ashx?op=keyword'

headers={
    "User-Agent":"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36"
}
mes= input('请输入一个城市:')
data={
    'cname':'' ,
    'pid':'',
    'keyword': mes,
    'pageIndex': '1',
    'pageSize': '10',
}
#发送请求
response=requests.post(url=post_url,headers=headers,data=data)
#获取响应
result=response.text
if not os.path.exists('./网络爬虫/sucai/KFC/'):
    os.makedirs('./网络爬虫/sucai/KFC/')
#持久化存储
with open('./网络爬虫/sucai/KFC/'+mes+'KFC','w',encoding='utf-8') as f:
    f.write(result)
```

## (三)数据解析

#### 3.0概述

回顾requests模块实现爬虫的步骤：

1. 指定url
2. 发情请求
3. 获取响应数据
4. 持久化存储

起始在持久化存储之前还有一步数据解析，需要使用聚焦爬虫，爬取网页中部分数据，而不是整个页面的数据。下面会学习到三种数据解析的方式。至此，爬虫流程可以修改为:

1. 指定url
2. 发起请求
3. 获取响应
4. **数据解析**
5. 持久化存储

数据解析分类：

- 正则
- bs4
- xpath

数据解析原理概述：

- 解析的局部的文本内容都会在标签之间或者标签对应的属性中进行存储
- 进行指定标签的定位
- 标签或者标签对应的属性中存储的数据值进行提取（**解析**）

#### 3.1正则表达式

```python
# 元字符：具有固定含义的特殊符号
#常用元字符
'''
.  匹配除换行符以外的任意字符
\w 匹配字母或数字或下划线
\s 匹配任意的空白符
\d 匹配数字
\n 匹配一个换行符
\t 匹配一个制表符

^  匹配字符串的开始
$  匹配字符串结尾

\W        匹配非字母或数字或下划线
\S        匹配非空白符
\D        匹配非数字
a|b       匹配字符a 或者字符b
()        匹配括号内的表达式，也表示一个组
[.....]   匹配字符组中的字符
[^......] 匹配除了字符串中的字符的所有字符
'''
#量词：控制前面的元字符出现的次数
'''
*        重复零次或更多次
+        重复一次或者更多次
？       重复零次或一次
{n}      重复n次
{n,}     重复n次或者更多次
{n,m}    重复n到m次
'''
#贪婪匹配和惰性匹配
'''
.*  贪婪匹配
.*? 惰性匹配
'''
```

#### 3.2re模块

```python
import re

 #findall:匹配字符串中所有的符合正则的内容【返回的是列表】
list =re.findall(r'\d+',"我的电话号码是：10086,我女朋友的电话是：10010")
print(list)

# #finditer: 匹配字符串中所有的内容【返回的是迭代器】,从迭代器中拿到内容需要.group()
it =re.finditer(r'\d+',"我的电话号码是：10086,我女朋友的电话是：10010")
for item in it:
     print(item)
     print(item.group())

# search 找到一个结果就返回，返回的结果是match对象，拿数据需要.group(),  【全文匹配】
s= re.search(r'\d+',"我的电话号码是：10086,我女朋友的电话是：10010")
print(s.group())

#match是从头匹配
s=re.match(r'\d+',"10086,我女朋友的电话是：10010")
print(s.group())

#预加载正则表达式
s='''
<div class='jay'><span id='1'>郭麒麟</span></div>
<div class='jj'><span id='2'>白鹿</span></div>
<div class='salry'><span id='3'>李一桐</span></div>
'''
#(?P<分组名字>正则) 可以单独从正则匹配的内容中进一步提取到内容
obj=re.compile("<div class='(?P<daihao>.*?)'><span id='(?P<number>.*?)'>(?P<name>.*?)</span></div>")
result=obj.finditer(s)
for item in result:
    print(item.group("daihao"))
    print(item.group("number"))
    print(item.group("name"))
  
```

**案例一**：豆瓣排行

```python
import csv

import requests
import re

url ="https://movie.douban.com/top250"
header={
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36"
}
# 获取响应
res=requests.get(url=url,headers=header)

content_page=res.text
# print(content_page)
#解析数据
obj=re.compile(r'<li>.*?<span class="title">(?P<name>.*?)</span>.*?'
               r'<p class="">.*?<br>(?P<time>.*?)&nbsp.*?<div class="star">.*?'
               r'<span class="rating_num".*?>(?P<grade>.*?)</span>.*?'
               r'<span>(?P<number>.*?)</span>'
               ,re.S)
#开始匹配
result=obj.finditer(content_page)
f=open("data.csv",mode="w",encoding='utf-8')
cswriter=csv.writer(f)

for item in result:
    # print(item.group("name"))
    # print(item.group("time").strip())
    # print(item.group("grade"))
    # print(item.group("number"))
    dic=item.groupdict()
    dic['time']=dic['time'].strip()
    cswriter.writerow(dic.values())
f.close()
print("over")
```

#### 3.3 bs4数据解析

**数据解析原理**

```
   1. 	标签定位，
   2. 提取标签，标签属性中存储的数据值
       实例化一个beautifulSoup对象，并且将页面源码数据加载到该对象 中
   3. 通过调用BeautifulSoup对象中相关属性或者方法进行标签定位和数据提取
```

**环境安装：**

```python
pip install bs4
pip install lxml
```

**如何实例化BeautifulSoup对象**

```
- from bs4 import BeautifulSoup
```

* 对象实例化：

  1. 将本地的html文档中的数据加载到该对象中

  ```python
  fp=open('../01_初识爬虫/周杰伦.html','r',encoding='utf-8')
  soup=BeautifulSoup(fp,'lxml')
  print(soup)
  ```

  2. 将互联网上获取的页面源码加载到该对象中

  ```python
  page_text=response.text
  soup=BeautifulSoup(page_text,'lxml')
  print(soup)
  ```

```python
* 提供的用于数据解析的方法和属性：
1. soup.tageName:返回的是文档中第一次出现的tageName对应的标签内容
2. soup.find():
    * find('tageName'):等同于soup.div
    * 属性定位：
        - soup.find('div',class_='song')
3. soup.find_all('a')#返回符合要求的所有的标签，返回列表
4. soup.select('.tang') #class_='tang'  返回复数，列表里
    * soup.select('某种选择器') 选择器可以是id,class ，标签。。。选择器
    * 层级选择器使用：
        * soup.select('.tang > ul > li >a')[0]  
        >    >表示一个层级
        * soup.select('.tang > ul > li a')[0]
        > 空格表示多个层级
5. 获取标签中的文本数据
    - soup.a.text /string/get_text()
    > text 和 get_text()：可以获取一个某标签中所有的文本内容
        string只可以获取该标签下面直系的文本内容
6. 获取标签中的属性值
    - soup.a['href']
```

**案例一**：爬取三国演义小说所有章节标题和章节内容

[三国演义](https://www.shicimingju.com/book/sanguoyanyi.html)

```python
import requests
from bs4 import BeautifulSoup
from fake_useragent import  UserAgent
 
if __name__ =='__main__':
    headers={
        "User-Agent":UserAgent().chrome
    }
 	get_url='https://www.shicimingju.com/book/sanguoyanyi.html'
    #发起请求，获取响应
    page_text=requests.get(url=get_url,headers=headers).text.encode('ISO-8859-1')
    #在首页中解析出章节标题和章节内容
    #1. 实例化BeautifulSoup对象，将html数据加载到该对象中
    soup=BeautifulSoup(page_text,'lxml')
    # print(soup)
    #2.解析章节标题和详情页的url
    list_data=soup.select('.book-mulu > ul > li')
    fp=open('./sanguo.text','w',encoding='utf-8')
    for i in list_data:
        title=i.a.text
        detail_url='https://www.shicimingju.com/'+ i.a['href']
        #对详情页的url发送请求，
        detail_text=requests.get(url=detail_url,headers=headers).text.encode('ISO-8859-1')
        detail_soup=BeautifulSoup(detail_text,'lxml')
        #获取章节内容
        content=detail_soup.find('div',class_='chapter_content').text

        #持久化存储
        fp.write(title+":"+content+"\n")
        print(title,'下载完成')
```

#### 3.4xpath数据解析

* 实例化etree对象，且将被解析的源码加载到该对象中
* 调用etree对象中的xpath方法，结合着xpath表达式实现标签定位和内容的捕获

**案例一**：彼岸图网

```python
import requests
from lxml import etree
import os

from fake_useragent import UserAgent
headers={
    "User-Agent":UserAgent().chrome
}
url='https://pic.netbian.com/4kdongman/'
#发请求，获取响应
response=requests.get(url=url,headers=headers)
page_text=response.text

#数据解析 src属性  alt属性
tree=etree.HTML(page_text)
list_data=tree.xpath('//div[@class="slist"]/ul/li')
#print(list_data)

#创建文件夹
if not os.path.exists('./PicLibs'):
    os.mkdir('./PicLibs')
for li in list_data:
    img_src='https://pic.netbian.com' + li.xpath('./a/img/@src')[0]
    img_name=li.xpath('./a/img/@alt')[0]+'.jpg'
    #print(img_name,img_src)
    #通用解决中文乱码的解决方案
    img_name=img_name.encode("iso-8859-1").decode('gbk')
    #请求图片，进行持久化存储
    img_data=requests.get(url=img_src,headers=headers).content
    img_path='PicLibs/'+ img_name
    with open(img_path,'wb') as fp:
        fp.write(img_data)
        print(img_name,'下载成功！！！！')
```

**案例二**：[发表情](#https://www.fabiaoqing.com/biaoqing/lists/page/1.html)

```python


import requests,os
from lxml import etree
from fake_useragent import UserAgent

def crawl(url):
    headers={
        "User-Agent":UserAgent().chrome
    }
    #获取页面响应信息
    page_text=requests.get(url,headers).text
    #解析表情包的详情页url
    tree=etree.HTML(page_text)
    list_data=tree.xpath('//div[@class="ui segment imghover"]/div/a')
    if not os.path.exists('表情包'):
        os.mkdir('表情包')
    for i in list_data:
        detail_url='https://www.fabiaoqing.com'+i.xpath('./@href')[0]
        # print(detail_url)
        #对详情页面url发起请求，获取响应
        detail_page_text=requests.get(detail_url,headers).text
        tree=etree.HTML(detail_page_text)
        #得到搞笑图片的地址，发起请求进行持久化存储
        detail_list_data=tree.xpath('//div[@class="swiper-wrapper"]/div/img/@src')[0]
        fp=detail_list_data.split('/')[-1]
        with open('表情包/'+fp, 'wb') as fp:
            fp.write(requests.get(detail_list_data).content)
            print(fp,'下载完了！！！')
#调用
crawl('https://www.fabiaoqing.com/biaoqing/lists/page/1.html')
```

**案例三**：绝对领域

```python
import  requests
from fake_useragent import  UserAgent
from lxml import  etree
import os
def crawl():
    url='https://www.jdlingyu.com/tuji'
    headers={
        "User-Agent":UserAgent().chrome
    }
    #获取页面代码
    page_text=requests.get(url=url,headers=headers).text
    #print(page_text)
    tree=etree.HTML(page_text)
    list_url=tree.xpath('//li[@class="post-list-item item-post-style-1"]/div/div/a')
    #print(list_url)


    for i in list_url:
        detail_link=i.xpath('./@href')[0]
        #print(detail_link)
        #对详情页url发请求，解析详情页图片
        detail_data=requests.get(detail_link,headers).text
        tree=etree.HTML(detail_data)
        #名称
        list_name=tree.xpath('//article[@class="single-article b2-radius box"]//h1/text()')[0]
        # 创建一个相册文件夹文件夹

        if not os.path.exists('绝对领域\\'+list_name):
            os.makedirs('绝对领域\\'+list_name)
        #图片链接
        list_link=tree.xpath('//div[@class="entry-content"]/p')

        for i in list_link:
            img_src=i.xpath('./img/@src')[0]
            # 图片名称
            pic_name=img_src.split('/')[-1]
            with open(f'绝对领域\\{list_name}\\{pic_name}','wb') as fp:
                fp.write(requests.get(img_src).content)
                print(list_name,'下载完成')


crawl()
```

**案例四**：爬取站长素材中免费简历模板

```python
# 爬取站长素材中免费简历模板    官网：https://sc.chinaz.com/
from lxml import etree
import requests,os
import time 
if __name__ == '__main__':
    url = 'https://sc.chinaz.com/jianli/free.html'
    headers = {
        "user-agent": "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36"
    }
    # 获取免费简历模板网页数据
    page_text = requests.get(url=url, headers=headers).text
    #print(page_text)
    #解析数据，获取页面模板对应详情页面的url
    tree=etree.HTML(page_text)
    list_data= tree.xpath('//div[@id="main"]/div/div')
    #print(list_data)
    if not os.path.exists('./网络爬虫/sucai/站长素材'):
        os.makedirs('./网络爬虫/sucai/站长素材')
    for i in list_data:
        #解析出先要的详情页面链接
        detail_url ='https:'+i.xpath('./a/@href')[0]
        #获取简历模板名字
        detail_name =i.xpath('./p/a/text()')[0]
        detail_name=detail_name.encode('iso-8859-1').decode('utf-8')#解决中文编码问题
        #print(detail_name)
        #print(detail_url)
        #对详情页url发送请求，进行下载
        detail_data_text=requests.get(url=detail_url,headers=headers).text
        #进行数据解析，得到下载地址
        tree=etree.HTML(detail_data_text)
        down_link= tree.xpath('//div[@class="clearfix mt20 downlist"]/ul/li[1]/a/@href')[0]
        #print(down_link)
        #最后对下载地址链接发送请求，获取二进制数据
        final_data= requests.get(url=down_link,headers=headers).content
        #print(final_data)
        file_path='./网络爬虫/sucai/站长素材/'+detail_name
        with open(file_path,'wb') as fp:
            fp.write(final_data)
            time.sleep(1)  #下载延时1秒，防止爬取太快
            print(detail_name+'下载成功！！！')
```

## (四)爬虫进阶

#### 4.0验证码识别

* 验证码是门户网站中采取的一种反扒机制
* 反扒机制：验证码，识别验证码图片中的数据，用于模拟登陆操作

#### 4.1 cookie

* http/https协议特性:无状态
* 没有请求到对应页面数据的原因：发起第二次基于个人主页请求的时候，服务器端并不知道该此请求是基于登陆状态下的请求
* **cookie:**

> 用来让服务器端记录客户端的相关信息

1. 手动处理：通过抓包工具获取cookie值，将该值封装到headers中 （不建议）
2. 自动处理： 进行模拟登陆post请求后，由服务器创建，

* session会话对象
  * 作用：1. 可以进行请求发送
    2.如果请求过程中产生了cookie，则该cookie会被自动存储/携带在该session对象中
* 使用：
  1. 创建session对象
  2. 使用session对象进行模拟登陆post请求的发送，（cookie就会被存储在session中）
  3. session对象对个人主页对应的get请求进行发送（携带了cookie）

```python
import requests


#会话
session=requests.session()
data={
    'loginName': '13467205064',
    'password': '123321wasd',
}
#1.登陆
url='https://passport.17k.com/ck/user/login'

rs=session.post(url=url,data=data)
#print(response.text)
#print(response.cookies)#看cookie
#2.拿数据
#使用携带cookie的session进行get请求发送
response=session.get('https://user.17k.com/ck/author/shelf?page=1&appKey=2406394919')
print(response.json())
```

**另一种携带cookie访问**

直接把cookie放在headers中（不建议）

```python
#另一种携带cookie访问
res=requests.get('https://user.17k.com/ck/author/shelf?page=1&appKey=2406394919',headers={
    "Cookie":"GUID=868e19f9-5bb3-4a1e-b416-94e1d2713f04; BAIDU_SSP_lcr=https://www.baidu.com/link?url=r1vJtpZZQR2eRMiyq3NsP6WYUA45n6RSDk9IQMZ-lDT2fAmv28pizBTds9tE2dGm&wd=&eqid=f689d9020000de600000000462ce7cd7; Hm_lvt_9793f42b498361373512340937deb2a0=1657699549; sajssdk_2015_cross_new_user=1; c_channel=0; c_csc=web; accessToken=avatarUrl%3Dhttps%253A%252F%252Fcdn.static.17k.com%252Fuser%252Favatar%252F08%252F08%252F86%252F97328608.jpg-88x88%253Fv%253D1657699654000%26id%3D97328608%26nickname%3D%25E9%2585%25B8%25E8%25BE%25A3%25E9%25B8%25A1%25E5%259D%2597%26e%3D1673251686%26s%3Dba90dcad84b8da40; sensorsdata2015jssdkcross=%7B%22distinct_id%22%3A%2297328608%22%2C%22%24device_id%22%3A%22181f697be441a0-087c370fff0f44-521e311e-1764000-181f697be458c0%22%2C%22props%22%3A%7B%22%24latest_traffic_source_type%22%3A%22%E7%9B%B4%E6%8E%A5%E6%B5%81%E9%87%8F%22%2C%22%24latest_referrer%22%3A%22%22%2C%22%24latest_referrer_host%22%3A%22%22%2C%22%24latest_search_keyword%22%3A%22%E6%9C%AA%E5%8F%96%E5%88%B0%E5%80%BC_%E7%9B%B4%E6%8E%A5%E6%89%93%E5%BC%80%22%7D%2C%22first_id%22%3A%22868e19f9-5bb3-4a1e-b416-94e1d2713f04%22%7D; Hm_lpvt_9793f42b498361373512340937deb2a0=1657700275"
})
print(res.json())
```

#### 4.2代理

> 破解封IP的各种反扒机制

* 社么是代理：
  1. 代理服务器
* 代理作用：
  * 突破自身IP访问的限制
  * 可以隐藏自身真实IP
* 代理相关网站：
  * 快代理
  * www.goubanjia.com
  * 西祠代理
* 代理IP类型：
  * http:应用到http协议对应的URL中
  * https:应用到https协议对应的URL中
* 代理IP的匿名度：
  * 透明：服务器知道该次请求使用了代理， 也知道请求对应的真实IP
  * 匿名：知道使用代理，不知道真实IP
  * 高匿名：不知道使用代理，更不知道真实IP

**案例**：

```python
import requests


proxies={
    #"http":""
    "https":"222.110.147.50:3218"  #代理IP地址
}
res=requests.get("https://www.baidu.com/s?tn=87135040_1_oem_dg&ie=utf-8&wd=ip",proxies=proxies)
res.encoding='utf-8'

with open('ip.html','w') as fp:
    fp.write(res.text)
```

#### 4.3防盗链

在访问一些链接的时，网站会进行**溯源**，

他所呈现的反扒核心：当网站中的一些地址被访问的时候，会溯源到你的上一个链接
需要具备的一些“环境因素”，例如访问的过程中需要请求的时候携带headers

**案例**:梨视频

```PYTHON
#分析
#请求返回的地址 和 可直接观看的视频的地址有差异#
#请求返回的数据中拼接了返回的systemTime值
#真实可看的视频地址中有浏览器地址栏的ID值
#解决办法，将url_no中的systemTime值替换为cont-拼接的ID值
#url='https://www.pearvideo.com/video_1767372

import requests,os

if not os.path.exists('./网络爬虫/sucai/梨video'):
        os.makedirs('./网络爬虫/sucai/梨video')


#梨视频爬取视频
url='https://www.pearvideo.com/video_1767372'
contId= url.split('_')[1]
headers={
    "User-Agent":"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36",
    "Referer":url,
}
videoStatusUrl=f'https://www.pearvideo.com/videoStatus.jsp?contId={contId}&mrd=0.6004271686556242'
res =requests.get(videoStatusUrl,headers=headers)
#print(res.json())
dic=res.json()
srcUrl=dic['videoInfo']['videos']['srcUrl']
systemTime=dic['systemTime']
srcUrl=srcUrl.replace(systemTime,f'cont-{contId}')
#下载视频，进行存储
video_data=requests.get(url=srcUrl,headers=headers).content
with open('./网络爬虫/sucai/梨video/video.mp4','wb')as fp:
    fp.write(video_data)
```

## (五)异步爬虫

目的：在爬虫中使用异步实现高性能的数据爬取操作

* 进程是资源单位，每一个进程至少要有一个线程
* 线程是执行单位
* 同一个进程中的线程可共享该进程的资源
* 启动每一个城西默认都会有一个主线程

**异步爬虫方式：**

1. 多线程，多进程：（不建议）

   * 好处：可以被相关阻塞的操作单独开启线程或者进程，阻塞操作就可以异步执行
   * 弊端：无法无限开启多线程或者多进程。
2. 进程池，线程池（适当使用）

   * 好处：可以降低系统对进程或者线程创建和销毁一个频率，从而很好的降低系统的开销
   * 弊端：池中线程或者进程的数量有上限
   * 线程池

> 一次性开辟一些线程，我们用户直接给线程池提交任务，线程任务的调度交给线程池来完成

3. 单线程+异步协程

* event_loop:事件循环，相当于一个无限循环，我们可以把一些函数注册到这个事件循环上
  当满足某些条件的时候，函数就会被循环执行
* coroutine:协程对象，我们可以将协程对象注册到事件循环中，它会被事件循环调用。
  我们可以使用 **async**关键字来定义一个方法，这个方法在调用时不会立即执行，而是返回一个协程对象
* task：任务，它是对协程对象的进一步封装，包含了任务的各个状态，
* future:代表将来执行或还没有执行的任务，实际上和task没有本质区别
* async:定义一个协程
* await用来挂起阻塞方法的执行

#### 5.0单线程

```python
headers={
    "User-Agent":"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36"
}

urls=[
    'https://www.51kim.com/'
    'https://www.baidu.com/'
      'https://www.baidu.com/'
]
```

#### 5.1多线程

```python
from threading import Thread  #线程类

def fun():
    for i in range(100):
        print('func',i)


if __name__ =='__main__':
    t=Thread(target=fun) #创建线程，为线程安排任务
    t.start()  #多线程状态为可以开始工作的状态，具体执行时间由CPU决定

    for i in range(100):
        print('main',i)
    
'''
def MyThread(Thread):
    def run(self):   #当线程被执行的时候，被执行的就是run
        for i in range(1000):
            print("子线程",i)
if __name__ =='__main__':
    t=MyThread()
    t.start()#开启线程
    for i in range(1000):
        print("主线程",i)

'''

def fun(name):
    for i in range(1000):
        print(name,i)


if __name__ =='__main__':
    #给进程起名字，分得清楚是哪个进程
    t1=Thread(target=fun,args=('周杰伦',)) #传递参数必须是元组
    t1.start()  

    t2=Thread(target=fun,args=('周星驰',)) 
    t2.start()  
```

#### 5.2多进程

```python
from multiprocessing import Process



def fun():
    for i in range(10000):
        print('子进程',i)

 
if __name__ =='__main__':
    p=Process(target=fun) 
    p.start() 

    for i in range(10000):
        print('主进程',i)
```

#### 5.3线程池和进程池

```python
from concurrent.futures import ThreadPoolExecutor,ProcessPoolExecutor


def fun(name):
    for i in range(1000):
        print(name,i)
if __name__ =='__main__':
    #创建线程池
    with ThreadPoolExecutor(50) as t:
        for i in range(100):
            t.submit(fun,name=f'线程{i}')

    #等待线程池中的任务全部执行完毕，才继续执行
    print('123132')

'''
def fun(name):
    for i in range(1000):
        print(name,i)
if __name__ =='__main__':
    #创建进程池
    with ProcessPoolExecutor(50) as t:
        for i in range(100):
            t.submit(fun,name=f'进程{i}')

    #等待线程池中的任务全部执行完毕，才继续执行
    print('123132')
    '''
```

**案例**：水果交易网

```python
#https://www.guo68.com/sell?page=1中国水果交易市场

#先考虑如何爬取单个页面的数据
#再用线程池，多页面爬取
import requests,csv,os
from fake_useragent import UserAgent
from concurrent.futures import ThreadPoolExecutor
from lxml import etree


if not os.path.exists('./网络爬虫/sucai/中国水果交易'):
    os.makedirs('./网络爬虫/sucai/中国水果交易')
f='./网络爬虫/sucai/中国水果交易/fruit.csv'
fp=open(f,'w',encoding='utf-8')
csvwrite=csv.writer(fp)


def download_one_page(url):
    #拿去页面代码
    response=requests.get(url=url,headers={"user-agent":UserAgent().chrome}).text
    #解析数据 
    tree=etree.HTML(response)
    list_data=tree.xpath('//li[@class="fruit"]/a')
    all_list_data=[]
    for i in list_data:
        list_price=i.xpath('./div[2]/span[1]/text()')[0]
        list_sort=i.xpath('./p[1]/text()')[0]
        list_address=i.xpath('./p[2]/text()')[0]
        all_list_data.append([list_price,list_sort,list_address])
        #print(list_price,list_address,list_sort)
    #持久化存储
    csvwrite.writerow(all_list_data)
    print(url,'下载完毕')

if __name__ =='__main__':
    with ThreadPoolExecutor(50) as Thread: #使用线程池，池子里放50个线程
        for i in range(1,60):
            Thread.submit(download_one_page,f'https://www.guo68.com/sell?page={i} ')
    print("全部下载完毕")
```

#### 5.4协程

* 协程不是计算机真实存在的（计算机只有进程，线程），而是由程序员人为创造出来的。
* 协程也可以被成为微线程，是一种用户态内的上下文切换技术，简而言之，其实就是通过一个线程实现代码块相互切换执行
* **实现协程方法：**

  * 通过greenlet，早期模块
  * yield关键字
  * asyncio模块
  * **async,await关键字** 【推荐】
* **协程意义**

  * 在一个线程中如果遇到IO等待时间，线程不会傻傻等，利用空闲的时间再去干点别的事

```python
import time 
def fun():
    print('*'*50)
    time.sleep(3)#让当前线程处于阻塞状态，CPU是不为我工作的
    print('*****'*10)

#input()  程序也是处于阻塞状态
# requests.get(bilibili)在网络请求返回数据之前，程序也是处于阻塞状态
# 一般情况下，当程序处于 IO操作的时候，线程都会处于阻塞状态

# 协程:当程序遇见IO操作的时候，可以选择性的切换到其他任务上
# 在微观上是一个任务一个任务的切换进行，切换条件一般是IO操作
# 宏观上，我们能看到的其实就是多个任务一起在执行
# 多任务异步操作

# 上方所讲都是在单线程条件下

if __name__ =='__main__':
    fun()
```

```python

#Python 协程
import asyncio
import time
'''
async def fun1():
    print("你好，我叫周星驰")
    time.sleep(3)  #当程序出现同步操作的时候，异步就中断了，requests.get
    await asyncio.sleep(3)  #异步操作代码
    print('我叫周星驰')

async def fun2():
    print("你好，我叫周杰伦")
    time.sleep(2)
    await asyncio.sleep(2)
    print('我叫周杰伦')


async def fun3():
    print("你好，我叫周星驰")
    time.sleep(4)
    await asyncio.sleep(4)
    print('我叫小潘')


if __name__ =="__main__":

    f1 =fun1()#此时的函数是异步协程函数，此时函数执行得到的是一个协程对象
    #print(g)
    #asyncio.run(g)  #协程需要asyncio模块的支持
    f2=fun2()
    f3=fun3()
    task=[
        f1,f2,f3
    ]
    t1=time.time()
    #一次性启动多个任务（协程）
    asyncio.run(asyncio.wait(task))
    t2=time.time()
    print(t2-t1)
'''

async def fun1():
    print("你好，我叫周星驰")  
    await asyncio.sleep(3)  #异步操作代码
    print('我叫周星驰')

async def fun2():
    print("你好，我叫周杰伦")
   
    await asyncio.sleep(2)
    print('我叫周杰伦')


async def fun3():
    print("你好，我叫周星驰")
   
    await asyncio.sleep(4)
    print('我叫小潘')


async def main():
    #第一种写法
    #fi=fun1()
    #await f1  #一般await挂起操作放在协程对象前面

    #第二种写法
    task=[

        asyncio.create_task(fun1()), #py3.8版本以后加上asyncio.create_task()
        asyncio.create_task(fun2()),
        asyncio.create_task(fun3())
    ]
    await asyncio.wait(task)
if __name__ =="__main__":
    t1=time.time()
    asyncio.run(main())
    t2=time.time()
    print(t2-t1)
  
```

##### 5.4.1async  & await关键字

python3.4之后版本能用

```python

import asyncio

async def fun1():
    print(1)
    await asyncio.sleep(2)#遇到IO操作时，自动切换到task中的其他任务
    print(2)
async def fun2():
    print(3)
    await asyncio.sleep(3)#遇到IO操作时，自动切换到task中的其他任务
    print(4)

task=[
    asyncio.ensure_future(fun1())
    asyncio.ensure_future(fun2())
]
loop= asyncio.get_event_loop()
loop.run_untile_complete(asyncio.wait(task))
#遇到IO阻塞自动切换

```

##### **5.4.2事件循环**

理解为一个死循环，检测并执行某些代码

```python
import asyncio
#生成或获取一个事件循环
loop=asyncio.get_event_loop()
#将任务放到‘任务列表’
loop.run_untile_complete(任务)
```

##### 5.4.3快速上手

协程函数：定义函数时，async def 函数名
协程对象：执行 协程函数()，得到的就是协程对象

```python
async def func():
    pass

result=func()

```

**执行协程函数创建协程对象，函数内部代码不会执行**
**如果想要执行协程函数内部代码，必须要将协程对象交给事件循环来处理**

```python
import asyncio

async def func():
    pritn("你好，中国)

result=func()

#loop=asyncio.get_event_loop()
#loop.run_untile_complete(result)

asyncio.run(result)   
```

##### 5.4.4await关键字

await +可等待对象（协程对象，task对象，Future对象=====》IO等待）

示例1：

```python

import asyncio
async def fun():
    print("12374")
    await asyncio.sleep(2)
    print("结束")

asyncio.run(fun())
```

示例2:

```python
import asyncio
async def others():
    print('start')
    await asyncio.sleep(2)
    print('end")
    return '返回值'

async def fun():
    print("执行协程函数内部代码")
    #遇到IO操作 挂起当前协程（任务），等IO操作完成之后再继续往下执行，当前协程挂起时，事件循环可以去执行其他些协程（任务）
    response =await others()

    print('IO请求结束，结果为：',response)


asyncio.run(fun())

```

示例3：

```python
import asyncio
async def others():
    print('start')
    await asyncio.sleep(2)
    print('end")
    return '返回值'

async def fun():
    print("执行协程函数内部代码")
    #遇到IO操作 挂起当前协程（任务），等IO操作完成之后再继续往下执行，当前协程挂起时，事件循环可以去执行其他些协程（任务）
    response1 =await others()
    print('IO请求结束，结果为：',response1)


    response2 =await others()
    print('IO请求结束，结果为：',response2)
asyncio.run(fun())

```

await  等待对象的值得到结果之后才继续往下走

##### 5.4.5Task对象

> 白话：在事件循环中添加多个任务
> Task用于并发调度协程，通 `asyncio.create_task(协程对象)`的方式创建Task对象，这样可以让协程加入事件循环中等待被调度执行，除了使用 `asyncio.create_task（）`函数以外，还可以用低级的 `loop.create_task()`或者 `ensure_future()`函数。不建议手动实例化Task对象

示例1：

```python
import asyncio

async def func():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return '返回值'
async def main():
    print("main开始")

    #创建Task对象，将当前执行func()函数任务添加到事件循环中
    task1=asyncio.create_task(func())
    #创建Task对象，将当前执行func()函数任务添加到事件循环中
    task2=asyncio.creat_task(func())

    print("main结束")
    #当执行某协程遇到IO操作时，会自动化切换执行其他任务
    #此处的await是等待相对应的协程全部都执行完毕并获取结果
    res1=await task1
    res2=await task2
    print(res1,res2)

asyncio.run(main())
```

示例2：

```python
import asyncio

async def func():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return '返回值'
async def main():
    print("main开始")

    task_list=[
    asyncio.create_task(func(),name="n1")
    asyncio.create_task(func(),name="n2")
]
    print("main结束")
   #返回值会放到done里面，
    done,pending=await asyncio.wait(task_list,timeout=None)
    print(done)

asyncio.run(main())
```

示例3：

```python
import asyncio

async def func():
    print(1)
    await asyncio.sleep(2)
    print(2)
    return '返回值'


task_list=[
        func(),
        func(),
]
  

done,pending=asyncio.run( asyncio.wait(task_list))
print(done)
```

##### 5.4.6async.Future对象

Task继承Tuture对象，Task对象内部await结果的处理基于Future对象来的
示例1：

```python
async def main():
    #获取当前事件循环
    loop=asyancio.get_running_loop()

    #创建一个任务（Future对象）这个任务什么都不干
    fut=loop.create_future()

    #等待任务最终结果（Future对象）,没有结果则会一直等下去
    await fut

asyncio.run(main())
```

示例2：

```python
async def set_after():
    await asyncio.sleep(2)
    fut.set_result("666")

async def main():
    #获取当前事件循环
    loop=asyancio.get_running_loop()

    #创建一个任务（Future对象）没绑定任何行为，则这个任务永远不知道社么时候结束
    fut=loop.create_future()

    #创建一个任务（Task对象）绑定了set_after函数，函数内部在2s后，会给fut赋值
    #手动设置future任务的最终结果，那么fut就可以结束了
    await loop.create_task(set_after(fut))
  
    #等待 Future对象获取 最终结果，否则一直等下去
    data=await fut
    print(data)
asyncio.run(main())

```

##### 5.4.7concurrent.futures.Future对象

使用线程池，进程池来实现异步操作时用到的对象

```python
import time 
from concurrent.futures.thread import ThreadPoolExecutor
from concurrent.futures.process import ProcessPoolExecutor
from concurrent.futures import Future

def fun(value):
    time.sleep(1)
    print(value)
pool=ThreadPoolExecutor(max_workers=5)
#或者pool=ProcessPoolExecutor(max_workers=5)

for i in range(10):
    fut =pool.submit(fun,i)
    print(fut)
```

##### 5.4.8异步和非异步

案例：asyncio + 不支持异步的模块

```python
import requests
import asyncio

async def down_load(url):
    #发送网络请求，下载图片，（遇到网络下载图片的IO请求，自动化切换到其他任务）
    print('开始下载',url)

    loop=asyncio.get_event_loop()
    #requests模块默认不支持异步操作，所以使用线程池来配合实现了
    future=loop.run_in_executor(None,requets.get,url)

    response=await future
    print("下载完成")
    #图片保存本地
    file_name=url.rsplit('/')[-1]
    with open(file_name,'wwb') as fp:
        fp.write(response.content)
if __name__=="__main__":
    urls=[
        ""
        ""
        ""
    ]
    tasks=[ down_load(url) for url in urls]+
    loop=asyncio.get_event_loop()
    loop.run_until_complete(asyncio.wait(tasks))
```

##### 5.4.9异步上下文管理器

#### 5.5 多任务异步协程

```python
import asyncio,time


async def request(url):
    print("正在下载：",url)
    await asyncio.sleep(2)
    print('下载完毕！！！',url)
strat_time=time.time()
urls=[
    "www.baidui.com",
    "www.sougou.com",
    "www.goubanjai.com"
]
tasks=[]
for url in urls:
    c =request(url)
    task=asyncio.ensure_future(c)
    tasks.append(task)

loop=asyncio.get_event_loop()
#需要将任务列表封装到waith中
loop.run_until_complete(asyncio.wait(tasks))

print(time.time()-strat_time)
```

#### 5.6aio模块

```python
#requests.get()#同步代码，——>异步操作aiohttp
#pip install aiohttp

import asyncio
import aiohttp
import aiofiles
urls=[
        "http://kr.shanghai-jiuxin.com/file/2020/0605/819629acd1dcba1bb333e5182af64449.jpg",
        "http://kr.shanghai-jiuxin.com/file/2022/0515/small287e660a199d012bd44cd041e8483361.jpg",
        "http://kr.shanghai-jiuxin.com/file/2022/0711/small72107f2f2a97bd4affd9c52af09d5012.jpg"
    ]

async def aiodownload(url):
   # s = aiohttp.ClientSession()  《=====》requests
    #requests.get .post
    #s.get  s.post
    img_name=url.split('/')[-1]
    async with aiohttp.ClientSession() as session:
        data=await session.get(url)
        async with aiofiles.open(img_name,'wb') as fp:
            fp.write(data) 
       # with open(img_name,'wb') as  fp:
            #fp.write(await data.content.read())#读取内容是异步的，需要await挂起
    print(img_name,'搞定')


async def main():
    tasks=[]
    for url in urls:
        tasks.append(aiodownload(url))
    await asyncio.wait(tasks)


    '''
    tasks=[]
    for url in urls
        task_obj=asyncio.ensure_future(download(url))
        tasks.append(task_obj)
  
    await asyncio.wait(tasks)
    '''
if __name__=="__main__":
    asyncio.run(main())
    #loop=asyncio.get_event_loop()
    #loop.run_until_complete(main())
```

## (六）动态加载数据处理

#### 6.0selenium模块使用

- 便捷获取网站动态加载的数据
- 实现模拟登陆

**selenium是基于浏览器自动化的一个模块**
**使用流程**

* 先打开chrome 输入 “chrome://version/”来查看chrome版本

[chromedriver驱动](http://npm.taobao.org/mirrors/chromedriver/)

[查看驱动和浏览器版本的映射关系](http://blog.csdn.net/huilan_same/article/details/51896672)

[chromedriver官方文档](https://chromedriver.chromium.org/home)

[浏览迷：各种浏览器版本](https://liulanmi.com/tag/chrome)

实例化一个浏览器对象：

```python
from selenium import webdriver
#实例化一个浏览器对象，（传入浏览器的驱动生成）
bro = webdriver.Chrome(executable_path = './chromedriver')
```

**方法**：

---

```python
#发起请求 get(url)
#标签定位 find_element  新版本需要导入from selenium.webdriver.common.by import By
#标签交互（搜索）send_keys('xxxx')
#执行js程序 excute_script(JS代码)
#前进，后退  fowawrd()  back()
#关闭浏览器  quit()
#对当前页面进行截图裁剪  保存， bro.save_screenshot('a.jpg')
```

**案例**：

```python
from selenium import webdriver
from lxml import etree
from time import sleep
#实例化浏览器驱动对象，
drive = webdriver.Chrome()
#让浏览器发送一个指定url请求
drive.get("https://www.shicimingju.com/book/hongloumeng.html")
#page_source获取浏览器当前页面的页面代码，（经过数据加载以及JS执行之后的结果的html结果），
#我们平时打开页面源代码看得数据 和  点击检查在elements中看得代码是不一样的，有些是经过动态加载，在页面源代码代码里面不显示
page_text=drive.page_source

#解析红楼梦章节标题
tree=etree.HTML(page_text)
list_data=tree.xpath('//div[@class="book-mulu"]/ul/li')
for i in list_data:
    article_title=i.xpath('./a/text()')[0]
    print(article_title)

sleep(5)
#关闭浏览器
drive.quit()
```

#### 6.1其他自动化操作

```python
from selenium import webdriver
from time import  sleep
from selenium.webdriver.common.by import By

drive=webdriver.Chrome()
drive.get('https://www.taobao.com/')
# print(drive.title)

#标签定位
search_input =drive.find_element(By.ID,'q')
#标签交互
search_input.send_keys('Iphone ')

#执行一组js程序
drive.execute_script("window.scrollTo(0,document.body.scrollHeight)")
sleep(3)
#点击搜索按钮
bottom = drive.find_element(By.CSS_SELECTOR,'.btn-search')
bottom.click()

#在访问一个url
drive.get('http://www.baidu.com')
sleep(2)
#回退
drive.back()
sleep(2)
#前进
drive.forward()
sleep(2)
# 关闭浏览器
sleep(4)
drive.quit()
```

#### 6.2拉钩网自动化操作

```python
from selenium.webdriver import Chrome
import  time
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By



drive = Chrome()
drive.get('https://www.lagou.com/')

#点击全国
quanguo=drive.find_element(by=By.XPATH,value='//*[@id="changeCityBox"]/p[1]/a')
quanguo.click()
#定位到搜索框, 输入python, 输入回车/点击搜索按钮
drive.find_element(by=By.XPATH,value='//*[@id="search_input"]').send_keys('Python',Keys.ENTER)

#得到职位，薪资，公司名称
div_list=drive.find_elements(by=By.XPATH,value='//*[@id="jobList"]/div[1]/div')
# print(div_list)
for i in div_list:
    Job_name=i.find_element(by=By.XPATH,value='./div[1]//a').text
    Job_price=i.find_element(by=By.XPATH,value='./div[1]//span').text
    company_name=i.find_element(by=By.XPATH,value='./div[1]/div[2]//a').text
    print(Job_name,Job_price,company_name)

time.sleep(2)
drive.quit()
```

#### 6.3窗口切换

```python
from selenium.webdriver import Chrome
import time
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

drive = Chrome()
drive.get('https://www.lagou.com/')

# 点击全国
drive.find_element(by=By.XPATH, value='//*[@id="changeCityBox"]/p[1]/a').click()
time.sleep(1)

# 定位到搜索框, 输入python, 输入回车/点击搜索按钮
drive.find_element(by=By.XPATH, value='//*[@id="search_input"]').send_keys('Python', Keys.ENTER)
time.sleep(1)

drive.find_element(by=By.XPATH, value='//*[@id="jobList"]/div[1]/div[1]/div[1]/div[1]/div[1]/a').click()

#如何进入到新窗口中进行提取
# 在selenium眼中，新窗口默认是不切换过来的
drive.switch_to.window(drive.window_handles[-1])

#在新窗口中提取内容
job_detail = drive.find_element(by=By.XPATH,value='//*[@id="job_detail"]/dd[2]').text
print(job_detail)

#关闭子窗口中
drive.close()

#变更窗口视角，回到原来的窗口
drive.switch_to.window(drive.window_handles[0])

print(drive.find_element(by=By.XPATH, value='//*[@id="jobList"]/div[1]/div[1]/div[1]/div[1]/div[1]/a').text)

time.sleep(2)
drive.quit()
```

#### 6.4动作链和iframe的处理

```python
from selenium.webdriver import Chrome
from selenium.webdriver.common.by import By
# 导入动作链对应的类
from selenium.webdriver import ActionChains
from time import sleep

bro = Chrome()
bro.get('')
# 如果定位的标签是存在于iframe标签中的则必须通过如下操作进行标签定位
bro.switch_to.frame(id)  # 切换浏览器标签定位的作用域

div = bro.find_element(By.ID, 'draggable')

#从iframe回到原页面用default_content()
# bro.switch_to.default_content() #切换到原页面


#动作链
action =ActionChains(bro)
#点击长按指定的标签
action.click_and_hold(div)#长按，

for i in range(5):
    #perform()立即执行动作链操作
    #move_by_offset(x,y)x表示水平方向，y表示竖直方向
    action.move_by_offset(17,0).perform()#移动
    sleep(0.3)
#释放动作链
action.release()

#关闭浏览器
bro.quit()
```

#### 6.5模拟登录qq空间

```python
from  selenium.webdriver import Chrome
from selenium.webdriver.common.by import By
from time import sleep
from selenium.webdriver import ActionChains

#创建浏览器对象
bro =Chrome()
#发送请求
bro.get('https://qzone.qq.com/')

#定位到密码登录标签，遇到iframe切换作用域
#切换作用域，定位标签
bro.switch_to.frame('login_frame')
a_tag=bro.find_element(By.ID,'switcher_plogin')
a_tag.click()

#定位输入账户的标签
username = bro.find_element(By.ID,'u')
#定位到输入密码的标签
password = bro.find_element(By.ID,'p')
#输入账户
username.send_keys('318129549')
sleep(1)
#输入密码
password.send_keys('song027..')
sleep(1)
#定位 登录 标签
login= bro.find_element(By.ID,'login_button')
#点击
login.click()
sleep(3)

bro.quit()
```

#### 6.6谷歌无头浏览器+反检测

```python
# 无可视化界面(无头浏览器)   phantomJs

from selenium.webdriver import Chrome
from time import sleep
#实现无可视化界面
from selenium.webdriver.chrome.options import Options
#实现规避检测
from selenium.webdriver import ChromeOptions


# 创建一个参数对象，用来控制chrome以无界面模式打开
chrome_options = Options()
chrome_options.add_argument('--headless')
chrome_options.add_argument('--dissable-gpu')
#实现规避检测
options=ChromeOptions()
options.add_experimental_option('excludeSwitches',['enable-outomation'])


bro = Chrome(chrome_options=chrome_options,options=options)

# 上网
bro.get('https://www.baidu.com')
print(bro.page_source)
sleep(2)

# 关闭浏览器
bro.quit()
```

## (七)scrapy框架

创建工程
scrapy startproject  xxx

进去到工程下
cd xxx

新建一个爬虫文件
scrapy genspider spiderName www.xxx.com

运行
scrapy crawl spiderName      (--nolog)

##### 7.0Scrapy持久化存储：

- 基于终端指令：
  - 要求：只可以将parse方法的返回值存储到本地的文本文件中
  - 要求：持久化存储对应的文本文件类型只可以是：json,jsonlines,jl,csv,xml,
  - 指令：scrapy crawl filename -o filepath
  - 缺点：局限性强，（数据只可以存储到指定后缀的文本文件中）
- 基于管道指令：
  - 编码流程：
    - 1. 数据解析
    - 2. 在item类中定义相关的属性
    - 3. 将解析的数据封装到item 类型的对象
    - 4. 将item类型对象提交给管道进行持久化存储的操作
    - 5. 在管道类的process_item中要将其接收到的item对象中存储的数据进行持久化存储操作
    - 6. 在配置文件中开启管道
  - 优点：通用性强。

1. 基于本地存储

   ```python
   基于终端指令存储只可以将parse方法的返回值存储到本地
   scrapy crawl douban. -o qidian.csv
   ```

```python
import scrapy
class DoubanSpider(scrapy.Spider):
    name = 'douban.'
    #allowed_domains = ['www.xxx.con']
    start_urls = ['https://www.qidian.com/all/']

    def parse(self, response):
        # 解析小说的作者和标题
        list_data=response.xpath('//div[@class="book-img-text"]/ul/li')
        alldata=[]
        for i in list_data:

            #xpath返回的是列表，但是列表里一定是Selector类型的对象
            #.extract()可以将Selector对象中的data存储的字符串提取出来
            #列表调用了extract()之后，则表示将列表中的每一个Selector对象中的data对应的字符串提取出来
            #.extract_first()将列表中第0个元素提取出来
            title =i.xpath('./div[2]/h2/a/text()').extract_first()
            author=i.xpath('./div[2]/p/a/text()')[0].extract()
      

             dic={
                 'title':title,
                 'author':author
             }
             alldata.append(dic)
         return  alldata
```

2.基于管道存储

2.1 爬虫文件的编写

```python
import scrapy
from douban.items import DoubanItem

class DoubanSpider(scrapy.Spider):
    name = 'douban.'
    #allowed_domains = ['www.xxx.con']
    start_urls = ['https://www.qidian.com/all/']

    def parse(self, response):
        # 解析小说的作者和标题
        list_data=response.xpath('//div[@class="book-img-text"]/ul/li')
   
        for i in list_data:
        
            title =i.xpath('./div[2]/h2/a/text()').extract_first()
            author=i.xpath('./div[2]/p/a/text()')[0].extract()
    
            item = DoubanItem()
            item['title'] = title
            item['author']=author

            yield item #将ltem提交给管道

```

2.2 items文件的编写

```python
import scrapy
class DoubanItem(scrapy.Item):
    # define the fields for your item here like:
    title = scrapy.Field()
    author=scrapy.Field()
    pass
```

2.3管道类的重写

```python
# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html


# useful for handling different item types with a single interface
from itemadapter import ItemAdapter
import pymysql


class DoubanPipeline:
    fp = None

    # 重写父类方法，该方法只在开始爬虫的时候只调用一次
    def open_spider(self, spider):
        print('开始爬虫')
        self.fp = open('./qidian.txt', 'w', encoding='utf-8')

    # 专门处理Item类型对象
    # 该方法可以接收爬虫文件提交过来的item对象
    # 该方法每接收一次Item就会被调用一次
    def process_item(self, item, spider):
        author = item['author']
        title = item['title']
        self.fp.write(title + ':' + author + '\n')
        return item#就会传递给下一个即将执行的管道类

    def close_spider(self, spider):
        print('结束爬虫')
        self.fp.close()

        # 管道文件中一个管道对应将一组数据存储到一个平台或者载体中


class mysqlPipeline:
    conn = None
    cursor = None

    def open_spider(self, spider):
        self.conn = pymysql.Connect(host='192.168.187.128', port=3306, user='root', password='13467205064', db='qidian',
                                    charset='utf8')

    def process_item(self, item, spider):
        self.cursor = self.conn.cursor()

        try:
            self.cursor.execute('insert into qidian values(0,"%s","%s")'%(item["title"],item["author"]))
            self.conn.commit()
        except Exception as e:
            print(e)
            self.conn.rollback()
        return item
    def close_spider(self, spider):
        self.cursor.close()
        self.conn.close()

        # 爬虫文件提交的item类型的对象最终会提交给哪一个管道类哪
```

##### 7.1基于Spider的全站数据解析爬取

- 就是将网站中某板块下的全部页码对应的页面数据进行爬取
- 自行手动进行请求发送数据（推荐）
  - 手动发送请求
    -yield scrapy.Resquest(url,callback) callback专门用作数据解析

**案例**：

```python
import scrapy
class QidianSpider(scrapy.Spider):
    name = 'qidian'
    #allowed_domains = ['www.xxx.com']
    start_urls = ['https://www.qidian.com/all/']
    #设置一个通用的url
    url='https://www.qidian.com/all/page%d/'
    page_num=2

    def parse(self, response):
        list_data=response.xpath('//div[@id="book-img-text"]/ul/li')
        for i in list_data:
            title =i.xpath('./div[2]/h2/a/text()').extract_first()
            author=i.xpath('./div[2]/p/a[1]/text()')[0].extract()
            styple=i.xpath('./div[2]/p/a[2]/text()').extract_first()
            styple1=i.xpath('./div[2]/p/a[3]/text()').extract_first()
            type=styple+'.'+styple1
            #print(title,author,type)

        if self.page_num<=3:
            new_url=format(self.url%self.page_num)
            self.page_num+=1
            #手动的发送请求callback回调函数是专门用于数据解析的
            yield scrapy.Request(url=new_url,callback=self.parse)

```

##### 7.2五大核心组件

1. 引擎 Scrapy

> 用来处理整个系统的数据流处理，触发事物（框架核心）

2. 调度器 Scheduler

> 用来接受引擎发过来的请求，压入队列中，并在引擎再次请求的时候返回，可以想象成一个URL（抓取网页的地址或者说是链接）的优先队列，由它来决定下一个要抓取的网址是什么，同时去除重复的网址

3. 下载器 Downloader

> 用于下载网页内容，并将网页内容返回给的链接（Scrapy下载器是建立在twisted这个高效的异步模型上的）

4. 爬虫 Spider

> 爬虫是主要干活的，用于从特定的网页中提取自己需要的信息，即所有的实体（item）用户也可以从中提取链接，让Scrapy继续抓取下一个页面

5. 项目管道 Pipeline

> 负责处理爬虫从网页中抽取的实体，主要功能是持久化存储，验证实体的有效性，清楚不许需要的信息，当页面被爬虫解析后，将被发送到项目管道，并经过几个特定的次序处理数据

##### 7.3请求传参

* 使用场景：如果爬取解析的数据不在同一张页面中（深度爬取）,
  将item 传递给请求所对应的回调函数
  需要将不同解析方法中获取的解析数据封装存储到同一个Item中，并且提交到管道

**案例**：起点小说网

1. 爬虫文件的编写

```python
import scrapy
from qidian2.items import Qidian2Item

class QidianSpider(scrapy.Spider):
    name = 'qidian'
    #allowed_domains = ['www.xxx.com']
    start_urls = ['https://www.qidian.com/all/']

    #解析详情页的信息
    def detail_parse(self,response):
        item=response.meta['item']
        detail_data=response.xpath('//div[@class="book-intro"]/p//text()').extract()
        detail_data=''.join(detail_data)
        item['detail_data']=detail_data

        yield item
        # print(detail_data)
    #解析小说名称和详情页的链接地址

    def parse(self, response):

        list_data=response.xpath('//div[@class="book-img-text"]/ul/li')
        for i in list_data:
            item = Qidian2Item()
            title=i.xpath('./div[2]/h2/a/text()').extract_first()
            item['title']=title
            detail_url='https:'+i.xpath('./div[2]/h2/a/@href').extract_first()
            #发起手动请求
            yield scrapy.Request(url=detail_url,callback=self.detail_parse,meta={'item':item})#请求传参，meta={},可以将meta字典传递给请求所对应的回调函数
```

2. items文件

```python
# Define here the models for your scraped items

# See documentation in:
# https://docs.scrapy.org/en/latest/topics/items.html
import scrapy

class Qidian2Item(scrapy.Item):
    # define the fields for your item here like:
    title= scrapy.Field()
    detail_data=scrapy.Field()
    pass
```

3. pipelines文件中管道类的重写

```python
# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html


# useful for handling different item types with a single interface
from itemadapter import ItemAdapter


class Qidian2Pipeline:
    fp=None
    def open_spider(self,spider):
        print('开始爬虫！！！！！！！！！！！！！')
        self.fp=open('./qidian.txt','w',encoding='utf-8')

    def process_item(self, item, spider):
        title=item['title']
        detail_data=item['detail_data']
        self.fp.write(title+":"+detail_data+"\n")
        return item

    def close_spider(self,spider):
        print('结束爬虫！！！！！！！！！！！！')
        self.fp.close()
```

##### 7.4图片数据爬取之ImagePipeline

```
* 基于scrapy爬取字符串类型数据和爬取图片类型数据区别：
    - 字符串： 只需要基于xpath 进行解析，且提交管道进行持久化存储
    - 图片： 只可以通过xpath解析出图片的src属性，单独的对图片地址发请求获取图片二进制类型的数据。
* 基于ImagePipeline：
    - 只需要将img的src属性值进行解析，提交到管道，管道就会对图片src进行发送请求获取图片二进制类型的数据，且还会帮我么进行持久化存储。
* 需求：爬取站长素材中高清图片
    - 使用流程：
        1. 数据解析（图片地址）
        2. 将存储图片地址的item提交到指定的管道类中
        3. 在管道文件中自定义一个基于ImagesPipeline的一个管道类
            - get_media_requests()
            - file_path()
            - item_completed()
        4. 在配置文件中：
            - 指定存储图片的目录：IMAGES_STORE='./imgs_站长素材'
            - 开启自定义的管道：
```

**1. 爬虫文件编写解析数据**

```python
#站长素材 高清图片的解析
import scrapy
from imgPro.items import ImgproItem

class ImgSpider(scrapy.Spider):
    name = 'img'
    #allowed_domains = ['www.xxx.com']
    start_urls = ['https://sc.chinaz.com/tupian/']

    def parse(self, response):
        div_list= response.xpath('//div[@id="container"]/div')
        for i in div_list:
            img_address='https:'+i.xpath('./div/a/img/@src').extract_first()
            #print(img_address)
            #实例化item对象
            item=ImgproItem()
            #传入img_address这个属性，
            item['img_address']=img_address
            #提交item 到管道
            yield item 

```

2.**把图片地址提交到管道**

```python
#来到item 中封装一个属性
scr=scrapy.Field()
```

3.**将解析到的数据封装到**item中

```python
from imgPro.items import ImgproItem
#实例化item对象
item=ImgproItem()
#传入img_address这个属性，
item['img_address']=img_address
#提交item 到管道
yield item 
```

4.**管道类的重写** pipelines

```python
import scrapy
from scrapy.pipelines.images import ImagesPipeline

class ImagsPipeline(ImagesPipeline):
    #可以根据图片地址进行图片数据的请求
    #对item中的图片进行请求操作
    def get_media_requests(self,item ,info ):
        yield scrapy.Request(item['src'])

    #指定图片持久化存储的路径
    def file_path(self,request,response=None,info=None):
        img_name=request.url.split('/')[-1]
        return img_name

    def item_completed(self,results,item ,info):
            return item #该返回值会传递一给下一个即将被执行的管道类

```

5.**图片存储路径**

```python
#指定图片存储目录
IMAGES_STORE='./imgs_站长素材'
```

6.**开启管道**

```python
#开启管道， 管道类名换成自定义的名称
ITEM_PIPELINES = {
    'imgPro.pipelines.ImagsPipeline': 300,
}
USER_AGENT = 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'

# Obey robots.txt rules
ROBOTSTXT_OBEY = False

#显示报错日志 只显示错误类型的日志信息
LOG_LEVEL="ERROR"
```