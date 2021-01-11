###### 1安装使用

```python
# 1.安装
pip install flask-restful

# 2.将views干掉,使用创建apis.py
# 这里访问的路由时/hello/ 返回的数据是json
from flask_restful import Api, Resource
api = Api()
def init_api(app):
  api.init_app(app)
class HelloResource(Resource):
  # 处理get请求
  def get(self):
    return {'msg': 'hello api'}
	# 处理post请求
  def post(self):
    return {'msg': 'post hello api'}
api.add_resource(HelloResource, '/hello/')

# 3. __init__.py中初始化apis
from flask import Flask
from .apis import init_api
from .ext import init_ext
from .settings import envs

def create_app(env):
  app = Flask(__name__)
  app.config.from_object(envs.get(env))
  init_ext(app)

  # 初始化restful的api
  init_api(app)

  return app
```

###### 2.将apis.py转成包的形式

```python
# 1.user_api.py
from flask_restful import Resource

class UsersResource(Resource):
  def get(self):
    # 传递过去的对象会变成json
    return {'msg': 'user_list'}

class UserResource(Resource):
  def get(self, id):
    return {'msg': 'user: ok %d' % id}

# 2.apis/__init__.py
from flask_restful import Api
from App.apis.user_api import UsersResource, UserResource

api = Api()
def init_api(app):
  api.init_app(app)

api.add_resource(UsersResource, '/users/')
api.add_resource(UserResource, '/user/<int:id>')

# 3.day6/__init__.py还是和原来一样
# 初始化路由
init_api(app)

# 4.访问
http://127.0.0.1:5000/users/
http://127.0.0.1:5000/user/1/
```

###### 3. restful格式化

```python

```

