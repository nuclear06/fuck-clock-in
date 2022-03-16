# BUPT 晨午晚检自动打卡

# 本程序仍在测试中，请不要随意使用

本程序会在北京时间`01:30、02:30、03:30`,`13:30、14:30、15:30`, `19:30、20:30、21:30`进行签到（考虑到github的action很不准，而且因为网络波动可能导致的打卡失败），并通过邮件返回是否打卡成功的信息,默认情况下打卡成功会返回填报的具体信息，失败会返回日志，对于前者可以修改`parameter.py`中的`RIGHT_RETURN = True`为`RIGHT_RETURN = False`，此时打卡成功仍会返回邮件，但是不会返回具体信息
\
（对于发送邮件的功能只测试了QQ邮箱，使用smtp+授权码发送邮件，理论上所有具有此功能的邮箱都可使用，相关内容参考QQ官方帮助文档https://service.mail.qq.com/cgi-bin/help?subtype=1&id=28&no=1001256 \
若要使用非QQ的邮箱，请自行修改`parameter`中的`port`为相应的smtp端口)

​			~~**本程序可以读取历史填报记录，并在指定时间进行打卡**~~

> 经实验发现服务器返回数据不全，且不会返回定位数据，于是最后干脆改成固定数据，既简单又方便~~绝对不是因为懒~~



`注意：本程序不能检测是否已经打卡`

> 本来写好了一个通过获得填报服务器的返回值进行判断函数，结果发现只要向填报的服务器发送请求就会算你填报上，哪怕你什么信息也没给他！(简直离谱)于是最后取消了本功能



教程:
先fork本项目，到自己账户的项目中

按照下图添加SECRET

![添加secret](images/添加secret.png)



## `USER`：

https://app.bupt.edu.cn/uc/wap/login/check	

该网站用于测试学号和密码是否正确

如果只有一个人，按如下模板填写：

```python
[
  {
    "user":"你的学号",
    "pswd":"你的密码",
    "id": "程序记录的名字（可以随便填）",
    "mail": 1,
    #	是否要邮件通知，[是]填1，[否]填0
    "mail_from": "发信邮件地址",
    "mail_key": "授权码",
    "mail_to": "发送到的邮件地址"
	#	这里发件地址和送件地址可以相同，即自己发给自己，如果不需要邮件通知可以不填写后三个参数
  }
]
```

多人模板：

```python
[
  {
    "user":"你的学号",
    "pswd":"你的密码",
    "id": "程序记录的名字（可以随便填）",
    "mail": 1,
    #	是否要邮件通知，[是]填1，[否]填0
    "mail_from": "发信邮件地址",
    "mail_key": "授权码",
    "mail_to": "发送到的邮件地址"
	#	这里发件地址和送件地址可以相同，即自己发给自己
  },
  {},{},{}
	#请用逗号分开每个大括号，大括号内的格式同单人
]

```



## `DATA`:

即你在打卡时发送的数据

https://app.bupt.edu.cn/site/ncov/xisudailyup

打卡网站，用于获得打卡数据

教程：

进入网站后填完数据，此时不要提交，按F12打开开发者模式，打开网络，此时进行提交，此时应会出现save项目，打开save项目，按如图操作复制得到数据

![获得DATA](https://github.com/nuclear06/fuck-clock-in/blob/2ec403d414b13abd279546260eff73f2ce267f9d/images/%E8%8E%B7%E5%BE%97DATA%20.png)




### 复制得到的数据如图

***注意：复制得到的数据请不要做任何修改,直接输入即可***
![example](images/example.png)

此时将图示数据填入`DATA`，若是多人填报时请用`回车/Enter`分隔不同数据 ，且每个数据与每个人要按顺序对应
