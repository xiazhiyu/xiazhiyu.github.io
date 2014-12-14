---
layout: post_cn
title: "用python构建restful服务"
date: 2014-12-14 21:31:36
categories: 开发
tag: python
---

跟java比起来python效率是很高的。

首先，通过virtualenv建立虚拟环境。
  {% highlight sh %}
virtualenv ENV
  {% endhighlight %}

进入创建好的ENV文件夹，启动虚拟环境。
  {% highlight sh %}
cd ENV
source ./bin/activate
  {% endhighlight %}

安装flask-restful。
  {% highlight sh %}
pip install flask-restful
  {% endhighlight %}

下面是示例代码：
  {% highlight python %}
#!bin/python
from flask import Flask, jsonify, abort, request, url_for, make_response
from flask.ext.httpauth import HTTPBasicAuth
from flask.ext.restful import Api, Resource, reqparse, fields, marshal
# try:
#     from flask.ext.cors import CORS  # The typical way to import flask-cors
# except ImportError:
#     # Path hack allows examples to be run without installation.
#     import os
#     parentdir = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
#     os.sys.path.insert(0, parentdir)

#     from flask.ext.cors import CORS
from flask.ext.cors import CORS

app = Flask(__name__)
# cors = CORS(app)
CORS(app, resources=r'/todo/*', headers='Content-Type')
api = Api(app)


# api.decorators=[cors.crossdomain(origin='*')]
auth = HTTPBasicAuth()

@auth.get_password
def get_password(username):
    if username == 'linkoubian':
        return '7c4a8d09ca3762af61e59520943dc26494f8941b'
    return None

@auth.error_handler
def unauthorized():
    return make_response(jsonify( { 'error': 'Unauthorized access' } ), 403)
    # return 403 instead of 401 to prevent browsers from displaying the default auth dialog

tasks = [
    {
        'id': 1,
        'title': u'Buy groceries',
        'description': u'Milk, Cheese, Pizza, Fruit, Tylenol',
        'done': False
    },
    {
        'id': 2,
        'title': u'Learn Python',
        'description': u'Need to find a good Python tutorial on the web',
        'done': False
    }
]

task_fields = {
    'id': fields.Integer,
    'title': fields.String,
    'description': fields.String,
    'done': fields.Boolean,
    'uri': fields.Url('task', absolute=True)
}

def make_public_task(task):
  new_task = {}
  for field in task:
      new_task[field] = task[field]
      if field == 'id':
          new_task['uri'] = url_for('get_task', task_id = task['id'], _external = True)
  return new_task

class TaskListAPI(Resource):
    def __init__(self):
        self.reqparse = reqparse.RequestParser()
        self.reqparse.add_argument('title', type = str, required = True, help = 'No task title provided2',  location = 'json')
        self.reqparse.add_argument('description', type = str, default = "", location = 'json')
        super(TaskListAPI, self).__init__()


    # decorators = [cors.crossdomain(origin='*')]

    def get(self):
        return {'tasks': tasks}

    def post(self):
        # print(self.reqparse)
        args = self.reqparse.parse_args()
        print args
        task = {
            'id': tasks[-1]['id'] + 1,
            'title': args['title'],
            'description': args['description'],
            'done': False
        }
        tasks.append(task)
        return { 'task': marshal(task, task_fields) } 

class TaskAPI(Resource):

    def __init__(self):
        self.reqparse = reqparse.RequestParser()
        self.reqparse.add_argument('title', type = str, location = 'json')
        self.reqparse.add_argument('description', type = str, location = 'json')
        self.reqparse.add_argument('done', type = bool, location = 'json')
        super(TaskAPI, self).__init__()

    @auth.login_required
    def get(self, id):
        task = filter(lambda t: t['id'] == id, tasks)
        if len(task) == 0:
            abort(404)
        return {'task': task[0]}

    def put(self, id):
        task = filter(lambda t: t['id'] == id, tasks)
        if len(task) == 0:
            abort(404)
        task = task[0]
        args = self.reqparse.parse_args()
        for k, v in args.iteritems():
            if v != None:
                task[k] = v
        return { 'task': marshal(task, task_fields) }

    def delete(self, id):
        pass

    def post(self):
        args = self.reqparse.parse_args()
        task = {
            'id': tasks[-1]['id'] + 1,
            'title': args['title'],
            'description': args['description'],
            'done': False
        }
        tasks.append(task)
        return { 'task': marshal(task, task_fields) } 

api.add_resource(TaskListAPI, '/todo/tasks', endpoint = 'tasks')
api.add_resource(TaskAPI, '/todo/tasks/<int:id>', endpoint = 'task')

if __name__ == '__main__':
    app.run(debug = True)
  {% endhighlight %}  
