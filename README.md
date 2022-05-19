# 在GitHub自我介绍

大家好！我是和平
虽然但是，我是一个游戏小主播
但是！我还是一个爱编程的小伙子！
我喜欢探究各种各样编程语言
比如：Python、HTML、CSS等

正在学HTML和CSS

这是我第一次写的markdown（~~不会用markdown的我是屑~~）

# 这是我做的能爬取B站视频的python小程序
我自己做了一个能爬取B站的视频python程序（~~其实是抄的~~）

但是我做了一个小改变，这个原来的程序是没有提醒“只供学习，禁止商用”的字样，我把它给添加上去了（当时我没有截图）

说一下原理
## 原理
	head = {    #自身伪造微软edge浏览器身份头向对方发送消息
	        "user-agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Edg/99.0.1150.46'
	    }
这个代码伪装自己是浏览器（如：Google Chrome, Microsoft Edge等）
简单来说就是**诈骗**（让我想起了那段**诈骗视频（Never gonna give you up）**）

就是简单的原理（~~爱整活的和平~~）

	box = easygui.enterbox('请输入你要下载视频的链接')
	baseurl = (box)
这段代码是我导入了一个模块，叫**easygui**

**注：这个模块是需要自己导入，如果没有这个模块，那么这个程序就是个摆设**

然后这个baseurl是给这段代码起个名字，可以随意更改，但是后面的代码需要对应

	title = doc('#viewbox_report > h1 > span').text()#获取视频标题
	pattern = r'\<script\>window\.__playinfo__=(.*?)\</script\>'
    result = re.findall(pattern, html)[0]
    temp = json.loads(result)
    #title为显示视频标题
    print(("开始下载--->")+title)
    print('下载速度是根据你家的网速有关')
    print('显示“视频下载完成”前请不要关闭程序')
这段代码是对应的视频连接（我也不知道叫什么。。。）

后面这三个print是打印，显示在控制台上
（我根本不会做网速提示啊啊啊啊啊）

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
由于B站的视频把每个视频和音频都拆开了，所以我们还需要下载音频
这个代码是下载音频（我也不会解释）

# 总结
这个程序可以爬取视频，我这个程序虽然免费开源，但是不要商用！我这个是免费开源，还有不允许盗取别人的视频！！！
请理解
