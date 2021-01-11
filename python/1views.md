###### 1.路由基础1

```python
from flask import Blueprint

blue = Blueprint('blue', __name__)

def init_view(app):
  app.register_blueprint(blue)
  return app

@blue.route('/')
def index():
  return 'Index'

# 接受路径参数,默认类型是string
@blue.route('/users/<id>/')
def users(id):
  print(id)
  print(type(id))
  return id

# conveter可以转换参数的类型  <int:id>  表示id的类型必须是int
@blue.route('/conveter/<int:id>/')
def conveter(id):
  print(id)
  print(type(id))
  return 'conveter suceess'

# string以斜线作为结尾
@blue.route('/getinfo/<string:token>')
def get_info(token):
  print(token)
  print(type(token))
  return 'gettoken success'

# path不以斜线作为结尾，可以匹配多个/参数
# http://127.0.0.1:5000/getpath/123/456/xxx
@blue.route('/getpath/<path:address>')
def get_path(address):
  print(address)            #
  print(type(address))
  return 'address success'

# uuid类型，限制只接受uuid
@blue.route('/getuuid/<uuid:uu>/')
def get_uuid(uu):
  print(uu)
  print(type(uu))
  return 'UUID success'

# any表示枚举， 只匹配
# http://127.0.0.1:5000/getany/a/
# http://127.0.0.1:5000/getany/b/
@blue.route('/getany/<any(a,b):an>/')
def get_any(an):
  print(an)
  print(type(an))
  return 'any success'

```

###### 2.不同的路由映射相同的函数

```python
# 两个不同的路由，可以映射到同一个函数（controller）处理
@blue.route('/first/')
@blue.route('/second/')
def first_second():
  return 'first or second'
```

###### 3.指定get和post

- 默认支持get，head, options 请求,发送post请求会报错,想支持某种请求需要手动注册

```python
# 后面加methods可以指定请求的方式
@blue.route('/first/', methods = ['GET', 'POST', 'DELETE'])
@blue.route('/second/', methods = ['GET', 'POST', 'DELETE'])
def first_second():
  return 'first or second'
```

###### 4.重定向

```python
# 重定向
@blue.route('/redirect/')
def red():
  # 1可以写路径
  # return redirect('/first/')

  # 2也可以反向解析，写视图函数的名字
  return redirect(url_for('blue.index'))
```

### Flask四大内置对象 request    session    g    config

###### 5.request对象（服务器在接收客户端请求后，会自动创建request对象，由flask对象创建，request对象不能修改）

```python
# request对象（直接从flask中导入request）
@blue.route('/getrequest', methods = ['GET', 'POST', 'DELETE'])
def get_request():
  print(request.host)
  print(request.url)
  print(request.path)
  if request.method == 'REQUEST':
    return 'request method'
  elif request.method == 'POST':
    return 'response method'
  else:
    return 'Request Success'
```

###### 6.request对象属性 

```python
url						完整请求地址
base_url			去掉GET参数的URL
host_url			只有主机和端口号的URL
path					路由中的路径
method				请求方法
remote_addr		请求的客户端地址
args					get请求参数
form					POST请求参数(直接支持put和patch请求)
files					文件上传
headers				请求头
cookies				请求中的cookie
```

###### 7.其他request属性

```python
@blue.route('/sendrequest/', methods=['GET', 'POST', 'PUT', 'DELETE'])
def send_request():

  print('===========================')

  # args获取request请求参数
  # 请求参数的类型为这种 ImmutableMultiDict([('age', '123'), ('name', 'hcp')])
  print(request.args)
  print(request.args.get('name'))
  print(type(request.args))

  # form获取POST请求参数   ImmutableMultiDict
  print(request.form)
  print(type(request.form))

  # 请求头   ImmutableMultiDict
  print(request.headers)
  print(type(request.headers))

  # 通过get获取ImmutableMultiDict中的数据
  Accepy_language = request.headers.get('Accept-Language')
  print(Accepy_language)
  return 'send success'
```

###### 8.渲染html模板

```python
@blue.route('/getresponse/')
def get_response():
  # Hello.html回去当前目录下的template目录找， 通过render_template可以渲染html模板
  result = render_template('Hello.html')
  print(result)
  print(type(result))
  return result


@blue.route('/getresponse/')
def get_response():
  # 手动创建response
  response = make_response('<h2>hello word</h2>')
  return response
```

###### 9.abort让请求终止

```python
@blue.route('/getresponse/')
def get_response():
  print('abort')
  # abort直接让请求终止,404会出现not found
  abort(404)

  # 手动创建response
  response = make_response('<h2>hello word</h2>')
  print(response)
  print(type(response))
  return response
```

###### 10.捕捉错误

```python
@blue.route('/getresponse/')
def get_response():
  # abort直接让请求终止
  abort(404)

# 捕捉404错误,捕捉之后不会显示404 not found, 而是会返回这里的404重定向到这里
@blue.errorhandler(404)
def handle_error(error):
  print(error)
  print(type(error))
  return '404重定向到这里'
```

###### 11.cookie(客户端会话技术,数据存储在客户端)

###### flask中的cookie默认对中文等进行了处理,直接可以使用中文

```python
# 1.设置cookie
@blue.route('/login', methods=['GET', 'POST'])
def login():
  if request.method == 'GET':
    return render_template('login.html')
  elif request.method == 'POST':
    username = request.form.get('username')
    # 创建response对象
    response = Response('登录成功 %s' % username)
    # 设置cookie
    response.set_cookie('username', username)
    return response

# 2.获取cookie
@blue.route('/getcookie/')
def get_cookie():
  # 获取cookie
  username = request.cookies.get('usernmae')
  return '欢迎回来 %s' % username
```

###### 12.session(session存储在服务端,session依赖于cookie)

```python
# 1.设置session
@blue.route('/login', methods=['GET', 'POST'])
def login():
  if request.method == 'GET':
    return render_template('login.html')
  elif request.method == 'POST':
    username = request.form.get('username')
    response = Response('登录成功 %s' % username)
    # 设置session
    session['username'] = username
    return response

# 2.获取session
@blue.route('/getsession/')
def get_session():
  # 获取session
  username = session.get('username')
  return '欢迎回来 %s' % username

# 3.需要在Config中配置SECRET_KEY否则设置session会报错
class Config:
  TESTING = False
  DEGUG = False
  SQLALCHEMY_TRACK_MODIFICATIONS = False
	# 设置SECRET_KEY 
  SECRET_KEY = 'flask'
```

###### 13.session2

```python
'''
	flask中将session存储在cookie中
	并且对数据进行了序列化
	还进行了base64编码
	还进行了zlib压缩
	还传递了hash
'''
```

###### 14.session持久化(存储在redis中)

###### 使用到的插件为flask-session ( 将数据存储在服务端，将数据对应的key存储在cookie中 )

```python
'''
redis安装
https://blog.csdn.net/NRlovestudy/article/details/89494939
'''
redis-server --service-start ( 启动服务 )
redis-server --service-stop（停止服务）

# 1.安装插件
pip install flask-session
pip install redis
# 2.配置类中加SESSION_TYPE = 'redis'
class Config:
  TESTING = False
  DEGUG = False
  SQLALCHEMY_TRACK_MODIFICATIONS = False
  SECRET_KEY = 'flask'
	# 配置类中加SESSION_TYPE = 'redis'
  SESSION_TYPE = 'redis'
  
# 3.ext.py中导入并初始化session
from flask_session import Session					#!!!!!
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
migrate = Migrate()

def init_ext(app):
  db.init_app(app)
  migrate.init_app(app,db)
  Session(app)													# !!!!

# 4.再设置session就会设置到redis中

# 5.redis中查看
> redis-cli
> keys *
```

###### 15.flask-session进阶(默认session存储在redis中为31天) 

- flask-session将数据存储在服务端，将数据对应的key存储在cookie中 

```python
class Config:
  TESTING = False
  DEGUG = False
  SQLALCHEMY_TRACK_MODIFICATIONS = False

  SECRET_KEY = 'flask'
	# !!!! session中的其他配置
  # session存储在redis中
  SESSION_TYPE = 'redis'
  # session是否安全，安全是通过SECRET_KEY做的
  SESSION_COOKIE_SECURE = True
  # 是否使用签名，使用签名之后客户端不能直接看到session
  SESSION_USE_SINGER = True
  # 连接redis的地址，默认是本机6379端口
  SESSION_REDIS = '127.0.0.1:6379'
```

###### 钩子函数

```python
app  	优先级更高
blue	默认只能处理本蓝图中的	
```









