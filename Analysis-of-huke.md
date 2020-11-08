import requests
import re
from lxml import etree
from datetime import datetime
import os
from time import sleep

User_Agent_pc = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/82.0.4050.0 Safari/537.36 Edg/82.0.423.0'
User_Agent_mobile = 'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1'

# 执行解析操作
def s_run(surl,Cookie,_csrf_frontend):
    headers_get_title = {
        'User-Agent': User_Agent_mobile
    }
    response = requests.get(url=surl, headers=headers_get_title)
    html = etree.HTML(response.text)
    title = html.xpath("//h1[@class='con']/text()")[0]


    headers = {
        'User-Agent':'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1',
        'Cookie': Cookie,
        'Host': 'm.huke88.com',
        'Referer': surl,
        'Accept': 'application/json, text/javascript, */*; q=0.01',
        'Origin': 'https://m.huke88.com',
        'X-Requested-With': 'XMLHttpRequest',
        'Sec-Fetch-Dest': 'empty',
        'Accept-Encoding':'gzip, deflate, br',
        'Sec-Fetch-Mode': 'cors',
        'Accept-Language':'zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6',
        'Content-Length':'116',
        'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8',

        'Sec-Fetch-Site': 'same-origin'
    }
    id = re.findall(r'\b\d+\b', surl)[0]
    data = {
        'id': id,
        '_csrf-frontend': _csrf_frontend
    }
    response = requests.post(url='https://m.huke88.com/video/video-url',headers=headers,data=data)
    videoUrl = response.json().get('data').get('videoUrl')

    infos = {
        'videoUrl': videoUrl,
        'title': title
    }
    print(infos)
    write(name=name,infos=infos)
    sleep(2)

# 解析操作2
def s_run2(surl,Cookie,_csrf_frontend):
    headers = {
        'User-Agent':User_Agent_mobile
    }
    surl = surl.replace('https://huke88.com/', 'https://m.huke88.com/')
    response = requests.get(url=surl, headers=headers)
    html = etree.HTML(response.text)
    title = html.xpath("//div[@class='list active']/a/p/text()")[0]

    headers = {
        'User-Agent':'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1',
        'Cookie': Cookie,
        'Host': 'm.huke88.com',
        'Referer': surl,
        'Accept': 'application/json, text/javascript, */*; q=0.01',
        'Origin': 'https://m.huke88.com',
        'X-Requested-With': 'XMLHttpRequest',
        'Sec-Fetch-Dest': 'empty',
        'Accept-Encoding':'gzip, deflate, br',
        'Sec-Fetch-Mode': 'cors',
        'Accept-Language':'zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6',
        'Content-Length':'116',
        'Content-Type':'application/x-www-form-urlencoded; charset=UTF-8',

        'Sec-Fetch-Site': 'same-origin'
    }
    career_id = re.findall(r'\b\d+\b', surl)[0]
    id = re.findall(r'\b\d+\b', surl)[1]
    data = {
        'id':id,
        'sourceIdentity':'0',
        'career_id': career_id,
        '_csrf-frontend': _csrf_frontend
    }
    response = requests.post(url='https://m.huke88.com/video/video-url',headers=headers,data=data)
    videoUrl = response.json().get('data').get('videoUrl')

    infos = {
        'videoUrl': videoUrl,
        'title': title
    }
    print(infos)
    write(name=name,infos=infos)
    sleep(2)

# 写入本地 bat 文件
def write(name,infos):
    with open(name,'a',encoding='utf-8') as f:
        f.write("TITLE" + " " + '"'+infos.get('title') +'"'+ '\n')
        f.write('"'+ r"N_m3u8DL-CLI_v2.7.5.exe" +'"'+ ' ' + '"' + infos.get('videoUrl') + '"' + ' ' + '--workDir' + ' ' + dir + ' ' + '--saveName' + ' ' + '"'+infos.get('title') +'"'+ ' '+ '--enableDelAfterDone ' '\n')

# 批量1
def Parse_page1(url,Cookie,_csrf_frontend):
    response = requests.get(url=url)
    html = etree.HTML(response.text)
    #First_dir = html.xpath("//h2/text()")[0]
    section_lists = html.xpath("//div[@class='item-tit']")[0]
    for les_item in section_lists:
        hrefs = les_item.xpath("//div[@class='cont-box']/div[@class='box-main']//a[@target='_blank']/@href")
        for href in hrefs:
            if href[0:7] != "https:/":
                href = "https://huke88.com" + href
            s_run(href,Cookie,_csrf_frontend)

# 批量2
def Parse_page2(url,Cookie,_csrf_frontend):
    response = requests.get(url=url)
    html = etree.HTML(response.text)
    srcs = html.xpath("//div[@class='dd-list']/a/@href")
    if srcs == []:
        srcs = []
        hrefs = html.xpath("//div[@class='right-mid']//a/@href")
        for href in hrefs:
            if href[0:5] == 'https':
                srcs.append(href)
    for src in srcs:
        s_run(surl=src, Cookie=Cookie, _csrf_frontend=_csrf_frontend)

# 批量3
def Parse_page3(url,Cookie,_csrf_frontend):
    response = requests.get(url=url)
    html = etree.HTML(response.text)
    hrefs = html.xpath("//div[@class='course-chapter']//a/@href")
    for href in hrefs:
        href = "https://huke88.com"+href
        s_run2(surl=href,Cookie=Cookie,_csrf_frontend=_csrf_frontend)

 # 得到几个重要参数
def get_frontend(url,Cookie):
    headers = {
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',
        'Cookie': Cookie,
        'Host': 'm.huke88.com',
        'Referer': 'https://huke88.com/',
        'Sec-Fetch-Dest': 'document',
        'Sec-Fetch-Mode': 'navigate',
        'Sec-Fetch-Site': 'same-site',
        'Sec-Fetch-User': '?1',
        'Upgrade-Insecure-Requests': '1',
        'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1'
    }

    response = requests.get(url=url, headers=headers)
    html = etree.HTML(response.text)
    token = html.xpath("//meta[@name='csrf-token']/@content")
    head = response.headers

    x = head.get('Set-Cookie')
    x = x.split(';')
    x = x[5]
    x = x.replace(' httponly, _csrf-frontend=', '').replace(
        '%3A2%3A%7Bi%3A0%3Bs%3A14%3A%22_csrf-frontend%22%3Bi%3A1%3Bs%3A32%3A%22', '').replace('%22%3B%7D', '')
    frontend = x[0:65]
    frontend2 = x[-32:]
    Cookie = Cookie + ';' + '_csrf-frontend={}%3A2%3A%7Bi%3A0%3Bs%3A14%3A%22_csrf-frontend%22%3Bi%3A1%3Bs%3A32%3A%22{}%22%3B%7D'.format(frontend,frontend2)
    return (Cookie,token)

 # 主执行命令
def run(url,Cookie):

    url = url.replace('https://huke88.com/', 'https://m.huke88.com/')
    if '_csrf-frontend=' not in Cookie:
        (Cookie, _csrf_frontend) = get_frontend(url=url, Cookie=Cookie)
        print(_csrf_frontend)
    (Ncookie, _csrf_frontend) = get_frontend(url=url, Cookie=Cookie)
    url = url.replace('m.huke88.com', 'huke88.com')

    global name
    name = datetime.now().strftime('%Y-%m-%d %H.%M.%S')
    name = str(name) + '.bat'
    with open(name, 'w', encoding='utf-8') as f:
        f.write("@echo off" + '\n' + "::Created by N_m3u8DL-CLI-SimpleG" + '\n' + r"chcp 65001 >nul" + '\n')
        global dir
        dir = r'./Downloads'
        if os.path.exists(dir) == False:
            os.makedirs('Downloads')
    a = url.split('/')
    if a[-2] == 'route':
        Parse_page1(url=url,Cookie=Cookie,_csrf_frontend=_csrf_frontend)
    elif a[-2] == 'video':
        Parse_page3(url,Cookie,_csrf_frontend)
        print('默认解析全部视频')
    else:
        print('下载全部请输入1，只下载一个请输入2')
        choice = input()
        if choice == '2':
            s_run(url, Cookie=Cookie, _csrf_frontend=_csrf_frontend)
        elif choice == '1':
            Parse_page2(url=url,Cookie=Cookie,_csrf_frontend=_csrf_frontend)
        else:
            print('输入错误！')
    raw = input('解析完成，按回车键退出……')

if __name__ == '__main__':
    Cookie = input('请输入Cookie:')
    while True:
        url = input('请输入url:')
        run(url=url, Cookie=Cookie)
