# 虎课网批量下载

一款可以批量下载虎课网视频的软件

# 前言

最近看到虎课网更新了，直接抓取的 m3u8 链接不能用了，目测是加密了。

#  分析

## 正常 UA 下，可以下载，不容易批量

以 https://huke88.com/course/28131.html 为例，抓到 m3u8 链接：`https://video-tx.huke88.com/cb3d3408vodtransgzp1256517420/dceb85805285890807324984997/voddrm.token.eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9~eyJ0eXBlIjoiRHJtVG9rZW4iLCJhcHBJZCI6MTI1NjUxNzQyMCwiZmlsZUlkIjoiNTI4NTg5MDgwNzMyNDk4NDk5NyIsImN1cnJlbnRUaW1lU3RhbXAiOjE2MDM4ODU2MjIsImV4cGlyZVRpbWVTdGFtcCI6MjE0NzQ4MzY0NywicmFuZG9tIjozMDE3Mjk5NDE1LCJvdmVybGF5S2V5IjoiMzNiMGJhN2U4NzFjYzczOWUwMmQ4YWE5YmUwYmI4NzAiLCJvdmVybGF5SXYiOiI1MGVlYTQ1ODg4NGIyNjY2M2Q4YjA5YzYyNzYwYmMwMSJ9~5XkltQzIkJx3oI96sQgfVrcW3xGcDPCQVlkZsLWOPp0.adp.12.m3u8?t=5f998466&rlimit=3&us=1603885622&sign=f4e25b158803bcc217441d87d4d5a92a`

`https://video-tx.huke88.com/cb3d3408vodtransgzp1256517420/dceb85805285890807324984997/voddrm.token.eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9~eyJ0eXBlIjoiRHJtVG9rZW4iLCJhcHBJZCI6MTI1NjUxNzQyMCwiZmlsZUlkIjoiNTI4NTg5MDgwNzMyNDk4NDk5NyIsImN1cnJlbnRUaW1lU3RhbXAiOjE2MDM4ODU2MjIsImV4cGlyZVRpbWVTdGFtcCI6MjE0NzQ4MzY0NywicmFuZG9tIjozMDE3Mjk5NDE1LCJvdmVybGF5S2V5IjoiMzNiMGJhN2U4NzFjYzczOWUwMmQ4YWE5YmUwYmI4NzAiLCJvdmVybGF5SXYiOiI1MGVlYTQ1ODg4NGIyNjY2M2Q4YjA5YzYyNzYwYmMwMSJ9~5XkltQzIkJx3oI96sQgfVrcW3xGcDPCQVlkZsLWOPp0.video_12_3.m3u8?us=1603885622&sign=f4e25b158803bcc217441d87d4d5a92a&rlimit=3&t=5f998466`

链接有时效性，不久就要失效，不过看链接有几个参数： us , sign , rlimit ,点击播放视频可以看得到，有一个 video-play 的 post 方法，

url: `https://asyn.huke88.com/video/video-play`

方法：`POST`

headers 里面有 

Host:`asyn.huke88.com`

Origen:`https://huke88.com`

Referer:`https://huke88.com/course/28131.html`

User-Agent:`Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/82.0.4050.0 Safari/537.36 Edg/82.0.423.0`

data 里面有

id: 28131

exposure: 1

studySourceId: 1

confirm: 0

async: false

_csrf-frontend: MTBFZHZ6RHgDch9cPCssHV4CEgAYFS4dbgk2EQ5MPTNUXiAxMR4HPw==

预览：

```json
{code: 4, msg: "可以播放,限制时间里重复播放", tx_file_id: "5285890807324984997", can_play: true, auth_type: 3,…}
app_id: 1256517420
auth_type: 3
can_play: true
catalog: [{courseTitle: "软件入门（旧）", courseId: "28131",…}, {courseTitle: "界面介绍&新建和存储（旧）", courseId: "28132",…},…]
catalogHeaderTitle: "PS-软件入门"
code: 4
courseId: "28131"
expire_at: 1603896605
msg: "可以播放,限制时间里重复播放"
nextCourse: {courseTitle: "界面介绍&新建和存储（旧）", courseId: "28132",…}
recommend: [{courseTitle: "22 角色和小场景元素结合（上色）",…}, {courseTitle: "08 人物半身像和场景元素搭配-上色篇",…},…]
recommendTeacherName: "阿角"
token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6MTI1NjUxNzQyMCwiZmlsZUlkIjoiNTI4NTg5MDgwNzMyNDk4NDk5NyIsImN1cnJlbnRUaW1lU3RhbXAiOjE2MDM4ODU4MDUsImV4cGlyZVRpbWVTdGFtcCI6MTYwMzg5NjYwNSwicGNmZyI6ImJhc2ljRHJtUHJlc2V0IiwidXJsQWNjZXNzSW5mbyI6eyJ0IjoiNWY5OTg1MWQiLCJybGltaXQiOjMsInVzIjoiMTYwMzg4NTgwNSJ9fQ.GL4tFyRXZP_nR8ZIKuGQHl-EkPRP90DgpBTrzLwr3tU"
tx_file_id: "5285890807324984997"
web_vvt_url: ""
```

key地址：`https://playvideo.qcloud.com/getplayinfo/v4/1256517420/5285890807324984997?psign=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHBJZCI6MTI1NjUxNzQyMCwiZmlsZUlkIjoiNTI4NTg5MDgwNzMyNDk4NDk5NyIsImN1cnJlbnRUaW1lU3RhbXAiOjE2MDM4ODU4MDUsImV4cGlyZVRpbWVTdGFtcCI6MTYwMzg5NjYwNSwicGNmZyI6ImJhc2ljRHJtUHJlc2V0IiwidXJsQWNjZXNzSW5mbyI6eyJ0IjoiNWY5OTg1MWQiLCJybGltaXQiOjMsInVzIjoiMTYwMzg4NTgwNSJ9fQ.GL4tFyRXZP_nR8ZIKuGQHl-EkPRP90DgpBTrzLwr3tU&overlayKey=b91b3f5ca5b0fb8faa452ae733d8419d&overlayIv=b8b11ba656409613833ce4491b9e18f3`

```json
{code: 0, message: "", requestId: "56abcfb9bd0b49f5bf3a01070c81faef", version: 4, context: "",…}
code: 0
context: ""
media: {basicInfo: {name: "PS-软件入门", size: 29630282, duration: 622, coverUrl: "", description: ""},…}
message: ""
requestId: "56abcfb9bd0b49f5bf3a01070c81faef"
version: 4
warning: ""
```

有key就可以下载，然而要实现批量不好弄。

## 手机模式的 UA ，可以下载，批量简单

同理，浏览器 UA 转到手机模式后同样找到 video-url

url:https://m.huke88.com/video/video-url

方法：POST

headers 如下：

​	

```json
	Accept: application/json, text/javascript, */*; q=0.01

​	Accept-Encoding: gzip, deflate, br

​	Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6

​	Content-Length: 115

​	Cookie:

​	Host: m.huke88.com

​	Origin: https://m.huke88.com

​	Referer: https://m.huke88.com/course/1991.html

​	Sec-Fetch-Dest: empty

​	Sec-Fetch-Mode: cors

​	Sec-Fetch-Site: same-origin

​	User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1

	X-Requested-With: XMLHttpRequest 
```

data 如下：

```json
id: 1991
_csrf-frontend: o8_TMOytDwkOKAJljMpa2Qt5xbmgXxTeWBZf4kwXcCnsveJIup9dXj1HRjbLjmmYeCm_0fMWU6wNRRKXYXkXaw==
```

很明显，要验证的东西少了很多，再看下这里的 m3u8 链接：

`https://m3u8.huke88.com/video/hls/v_1/2019-09-04/F7593C9C-F8E9-5D54-90F3-3087977613AD.m3u8?pm3u8/0/deadline/1603901153&e=1603890353&token=HUwgVvJnrW6fXOzqd_myfnE3FFoFLWJnNktg7ThD:W0xdI71w63nlQhmBMG9ZlAiLb_k=`

m3u8 也少了很多，根据多次对比发现，这里只有2个需要验证，deadline 为失效时间， token 为验证vip已购买用户，

```json
{code: 200, msg: "可以播放,因为你是全站VIP啊!", data: {,…}}
code: 200
data: {,…}
code: 1
confirm: 0
videoUrl: "https://m3u8.huke88.com/video/hls/v_1/2019-09-04/F7593C9C-F8E9-5D54-90F3-3087977613AD.m3u8?pm3u8/0/deadline/1603901153&e=1603890353&token=HUwgVvJnrW6fXOzqd_myfnE3FFoFLWJnNktg7ThD:W0xdI71w63nlQhmBMG9ZlAiLb_k="
msg: "可以播放,因为你是全站VIP啊!"
```

这是 video-url 的返回内容，与 UA 为 PC 下的返回内容有很大区别，所以这里肯定是两个人写的！

经测试这里的 m3u8 可以直接下载！

# 代码实现

这是我2020.10.28第二版代码，基本实现了所有功能，只做解析，生成的 bat 文件可以调用 https://github.com/nilaoda/N_m3u8DL-CLI/releases v2.7.5 版本，实现下载

```python
import requests
from lxml import etree
import re
import os
from datetime import datetime
from time import sleep
headers_mobile = {
    'User-Agent':'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1'
   }

# 实现单个视频下载
def single_run(url2,Cookie,_csrf_frontend):
    url1 = 'https://m.huke88.com/video/video-url'
    id = re.findall(r'\b\d+\b', url2)[0]
    headers1 = {
        'Cookie':Cookie,
        'Host': 'm.huke88.com',
        'Referer': url2,
        'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1',
        'Accept': 'application/json, text/javascript, */*; q=0.01',
        'Origin': 'https://m.huke88.com',
        'X-Requested-With': 'XMLHttpRequest',
        'Sec-Fetch-Dest': 'empty',
        'Sec-Fetch-Mode': 'cors',
        'Sec-Fetch-Site': 'same-origin'
    }
    data = {
        'id': id,
        '_csrf-frontend': _csrf_frontend
    }


    response = requests.post(url=url1, headers=headers1, data=data)
    videoUrl = response.json().get('data').get('videoUrl')


    response = requests.get(url=url2, headers=headers_mobile)
    html = etree.HTML(response.text)
    title = html.xpath("//h1[@class='con']/text()")[0]

    infos = {
        'videoUrl':videoUrl,
        'title':title
    }
    print(infos)
    write(infos)
    sleep(1)

def Parse_page(url,Cookie):
    response = requests.get(url=url)
    html = etree.HTML(response.text)
    #First_dir = html.xpath("//h2/text()")[0]
    section_lists = html.xpath("//div[@class='item-tit']")[0]
    for les_item in section_lists:
        hrefs = les_item.xpath("//div[@class='cont-box']/div[@class='box-main']//a[@target='_blank']/@href")
        for href in hrefs:
            if href[0:7] != "https:/":
                href = "https://huke88.com" + href
            single_run(href,Cookie,_csrf_frontend)

# 只做解析，生成 bat 文件，用 nilaoda 的下载器下载即可
def write(infos):
    try:
        with open(name,'a',encoding='utf-8') as f:
            f.write("TITLE" + " " + '"' + infos.get('title') +'"'+ '\n')
            f.write('"' + "N_m3u8DL-CLI_v2.7.5.exe" + '"' + ' ' + '"' + infos.get(
        'videoUrl') + '"' + ' ' + '--workDir' + ' ' + dir + ' ' + '--saveName' + ' ' + infos.get('title') + '\n')
    except:
        pass

def Parse_page2(url,Cookie,_csrf_frontend):
    headers = {
        'Cookie':Cookie
    }
    response = requests.get(url=url,headers=headers)
    html = etree.HTML(response.text)
    srcs = html.xpath("//div[@class='dd-list']/a/@href")
    if srcs == []:
        srcs = []
        hrefs = html.xpath("//div[@class='right-mid']//a/@href")
        for href in hrefs:
            if href[0:5] == 'https':
                srcs.append(href)
    for src in srcs:
        single_run(url2=src, Cookie=Cookie, _csrf_frontend=_csrf_frontend)

if __name__ == '__main__':
    print('请输入网址，例如：https://huke88.com/route/ae.html 或 https://huke88.com/course/3238.html')
    url = input('请输入网址：')
    url = url.replace('m.huke88.com','huke88.com')
     #Cookie 需要改 UA 再获取
    Cookie = input('请输入Cookie:')
    # video-play 里面可以看到
    _csrf_frontend = input('请输入_csrf-frontend：')
    a = url.split('/')
    name = datetime.now().strftime('%Y-%m-%d %H.%M.%S')
    name = str(name) + '.bat'
    with open(name,'w',encoding='utf-8') as f:
        f.write('@echo off' + '\n' + '::Created by N_m3u8DL-CLI-SimpleG' + '\n' + r'chcp 65001 >nul' + '\n')
        dir = r'./Downloads'
        if os.path.exists(dir) == False:
            os.makedirs('Downloads')

    if a[-2] == 'route':
        Parse_page(url, Cookie)

    else:
        print('检测到多个视频，下载全部请输入1，只下载一个请输入2')
        choice = input()
        if choice == '2':
            single_run(url, Cookie=Cookie, _csrf_frontend=_csrf_frontend)

        elif choice == '1':
            Parse_page2(url, Cookie,_csrf_frontend=_csrf_frontend)

        else:
            print('输入错误！')


```

更新链接：https://aohua.lanzoui.com/b00tyavza 密码:dzmi ，或在 github 下载 https://github.com/Nchujx/Analysis-of-huke

# 使用方法

1. 打开软件，输入网址

2. 浏览器 F12 切换 UA 到手机模式，刷新

3. 播放视频，点开 video-url ,获取里面的 Cookie 及 **_csrf-frontend**

4. 等待解析完成，解析完毕后双击生成的 bat 文件即可


