# python-
我的python代码作品，免费开源，你值得拥有
import requests
from requests import RequestException
from lxml import etree
from contextlib import closing
from pyquery import PyQuery as pq
import re
import os
import json
import subprocess
import easygui
import time

head = {    #自身伪造微软edge浏览器身份头向对方发送消息
        "user-agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edg/99.0.1150.46'
    }
#信息展示框
eula = easygui.ccbox(msg="注意！本程序为学习原理使用！如果销售！你自己承担！制作软件作者概不负责！！！",title="使用协议",choices=("同意","拒绝"))
if eula == 1:
    
    #baseurl是视频的来源链接，如果想要下载其他视频请把视频链接复制到baseurl
    box = easygui.enterbox('请输入你要下载视频的链接')
    baseurl = (box)
    #获取视频地址响应状态码
    html = requests.get(url = baseurl, headers = head).text
    doc = pq(html)
    title = doc('#viewbox_report > h1 > span').text()#获取视频标题
    pattern = r'\<script\>window\.__playinfo__=(.*?)\</script\>'
    result = re.findall(pattern, html)[0]
    temp = json.loads(result)
    #title为显示视频标题
    print(("开始下载--->")+title)
    print('下载速度是根据你家的网速有关')
    print('显示“视频下载完成”前请不要关闭程序')
    #导入urllib3模块
    import urllib3
    urllib3.disable_warnings()

    headers = {#自身伪造Microsoft Edge浏览器
            'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edg/99.0.1150.46',
            'Accept':'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9',#接收数据
            'Accept-Encoding':'gzip, deflate, br',#接受编码
            'Accept-Language':'zh-CN,zh;q=0.9,en;q=0.8'#接受语言
        }
    headers.update({'Referer': baseurl})
    res = requests.Session()
    begin = 0
    end = 1024*1024 - 1
    flag = 0
    #在当前目录以.flv视频格式保存下来
    filename = "./"+title+".flv"
    url = temp["data"]["dash"]["video"][0]['baseUrl']
    while True:
        headers.update({'Range': 'bytes=' + str(begin) + '-' + str(end)})
        res = requests.get(url=url, headers=headers,verify=False)
        if res.status_code != 416:
            begin = end + 1
            end = end + 1024*1024
        else:
            headers.update({'Range': str(end + 1) + '-'})
            res = requests.get(url=url, headers=headers,verify=False)
            flag=1
        with open(filename, 'ab') as fp:
            fp.write(res.content)
            fp.flush()
        if flag==1:
            fp.close()
            break

    print('--------------------------------------------')
    print('视频下载完成')
    print('--------------------------------------------')
    print('正在下载音频，请勿关闭程序')
    #在当前目录以.mp3音频格式保存下来
    filename = "./"+title+".mp3"
    url = temp["data"]["dash"]["audio"][0]['baseUrl']
    while True:
        headers.update({'Range': 'bytes=' + str(begin) + '-' + str(end)})
        res = requests.get(url=url, headers=headers,verify=False)
        if res.status_code != 416:
            begin = end + 1
            end = end + 1024*1024
        else:
            headers.update({'Range': str(end + 1) + '-'})
            res = requests.get(url=url, headers=headers,verify=False)
            flag=1
        with open(filename, 'ab') as fp:
            fp.write(res.content)
            fp.flush()
        if flag==1:
            fp.close()
            break

    print('音频下载完成')
    print('所有任务全部完成，现在可以放心大胆关闭了')
#如果你没有同意使用协议，将会显示拒绝后的信息并停止运行程序
else:
    easygui.msgbox('你拒绝了用户协议，本程序已停止运行')
