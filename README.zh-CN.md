# gohs-Ladon
[![Build Status](https://travis-ci.org/DigDeeply/gohs-ladon.svg?branch=master)](https://travis-ci.org/DigDeeply/gohs-ladon)    
基于Intel开源的hyperscan实现的GO版本的一个服务，取名拉冬，希腊神话中表示百头巨龙。给定一行文本，能够从海量的正则表达式中快速查询出命中了哪些正则，还可以返回该正则附加的一些数据。

例如，给出一个正则文本:
第一列是唯一id, 第二列是正则表达式, 第三列是附加数据
```
1	^[你|叫|什么|的|是]*名字[你|叫|什么|的|是]*$	{"type:"name", "user":"you"}
2	^[唱|一首|来]*歌[曲|吧|啊]*$	{"type":"song", "name":"random"}
3	^[唱|一首|来]*东风破[的|歌|歌曲|吧|啊]*$	{"type":"song", "name":"东风破"}
```
启动服务
```sh
./gohs-ladon --filepath=patterns/pattern2.txt
[2017-12-20T06:50:50Z] Hs-service 0.0.1 Running on 0.0.0.0:8080
```
通过服务查询
```
curl "http://127.0.0.1:8080/?q=你叫什么名字"

返回json,表明命中了第一条正则，并返回了正则文件中的附加数据
{
    "Errno": 0,
        "Msg": "",
        "Data": [
        {
            "Id": 1,
            "From": 0,
            "To": 18,
            "Flags": 0,
            "Context": "你叫什么名字",
            "RegexLinev": {
                "Expr": "^[你|叫|什么|的|是]*名字[你|叫|什么|的|是]*$",
                "Data": "{\"type:\"name\", \"user\":\"you\"}"
            }
        }
    ]
}
```
