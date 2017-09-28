pyhton 发qq邮件
```
#!/usr/bin/env python

import os, sys
import smtplib
from smtplib import SMTP_SSL
from email.header import Header
from email.mime.text import MIMEText
import datetime

def execCmd(cmd):
    r = os.popen(cmd)
    text = r.read()
    r.close()
    return text

now = datetime.datetime.now()



mailInfo = {
"from":"123123@qq.com",
"to":"yjc@qq.com",
"hostname":"smtp.qq.com",
"username":"123123@qq.com",
"password":"aa1234",
"mailsubject":"IP Address of fira linux",
"mailtext": now.strftime('%Y-%m-%d %H:%M:%S'),
"mailencoding":"utf-8"
}

if __name__ == '__main__':
    smtp = SMTP_SSL(mailInfo["hostname"])
    smtp.set_debuglevel(1)
    smtp.ehlo(mailInfo["hostname"])
    smtp.login(mailInfo["username"],mailInfo["password"])
    
    msg = MIMEText(mailInfo["mailtext"])
    msg["Subject"] = Header(mailInfo["mailsubject"],mailInfo["mailencoding"])
    msg["from"] = mailInfo["from"]
    msg["to"] = mailInfo["to"]
    smtp.sendmail(mailInfo["from"], mailInfo["to"], msg.as_string())
    
    smtp.quit()
```
