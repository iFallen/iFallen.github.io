---
layout: post
title: 通过Python挂一个简单的JSON测试接口 
excerpt: "Test:通过Python挂一个简单的JSON测试接口 Python/Flask/jsonify"
tags: [Python,JSON]
comments: true
---

[版权所有,转载请注明出处!](http://ifallen.github.com)


## `通过Python挂一个简单的JSON测试接口`
---


#### `前置条件`



- [`Flask 环境(点击跳转)`](http://flask.pocoo.org/docs/0.10/installation/#installation)

- `jsonify`

>
pip install jsonify

---

>
`test.py`

```
# -*- coding: UTF-8 -*-
# or #coding=utf-8 
from flask import Flask, request, jsonify

app = Flask(__name__)

tasks = [
    {
        'imageUrl': "http://git.oschina.net/qcrm/ImageTemp/raw/master/1.jpg",
        'title': u'Buy groceries',
        'description': u'uy groceries DetailInfo'
    },
    {
        'imageUrl': "http://git.oschina.net/qcrm/ImageTemp/raw/master/1.jpg",
        'title': u'Buy groceries',
        'description': u'uy groceries DetailInfo'
    },
    {
        'imageUrl': "http://git.oschina.net/qcrm/ImageTemp/raw/master/1.jpg",
        'title': u'Buy groceries',
        'description': u'uy groceries DetailInfo'
    },
    {
        'imageUrl': "http://git.oschina.net/qcrm/ImageTemp/raw/master/2.jpg",
        'title': u'Learn Python',
        'description': u'Need to find a good python tutorial on the web'
    }]


@app.route('/api/list', methods=['GET', 'POST'])
def get_tasks():
    return jsonify({'data': tasks})
    # if request.method == 'POST':
    #     return jsonify({'tasks':tasks})
    # else:
    #     return 'request method not allow'


@app.route('/api/login', methods=['POST'])
def login():

    name = request.form.get("name", None)
    password = request.form.get("password", None)   
    
    if name == None or password == None:
        return jsonify({'data': {'success': 0, 'error': '缺少必选参数'}})
    elif name == "admin" and password == "admin":
        return jsonify({'data': {'success': 1, 'error': ''}})
    else:
        return jsonify({'data': {'success': 0, 'error': "用户名或密码错误"}})


if __name__ == '__main__':
    app.run(host='0.0.0.0', debug=True)


```

---


>
运行
>
python test.py

---

>
终端，curl 测试 
>
curl -d "name=admin&password=admin" "127.0.0.1:5000/todo/api/login"
	
