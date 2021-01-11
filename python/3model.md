###### 1.使用flask-sqlalchmy插件

```python
pip install flask-sqlalchmy
```

###### 2.settings.py中配置数据库相关信息

```python
def get_db_uri(dbinfo):
  engine = dbinfo.get('ENGINE')
  driver = dbinfo.get('DRIVER')
  user = dbinfo.get('USER')
  password = dbinfo.get('PASSWORD')
  host = dbinfo.get('HOST')
  port = dbinfo.get('PORT')
  name = dbinfo.get('NAME')
  return '{}+{}://{}:{}@{}:{}/{}'.format(engine, driver, user, password, host, port, name)


class Config:
  DEBUG = False
  TESTING = False
  SQLALCHEMY_TRACK_MODIFICATIONS = False

  SECRECT_KEY = 'promise,then.catch'

class DevelopConfig(Config):
  DEBUG = True
  dbinfo = {
    'ENGINE': 'mysql',
    'DRIVER': 'pymysql',
    'USER': 'root',
    'PASSWORD': '123456',
    'HOST': '127.0.0.1',
    'PORT': '3306',
    'NAME': 'FLASK'
  }

  SQLALCHEMY_DATABASE_URI = get_db_uri(dbinfo)

envs = {
  'develop': DevelopConfig,
  'default': DevelopConfig
}
```

###### 3.App / ext.py文件中进行插件的初始化

```python
from flask_migrate import Migrate
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
migrate = Migrate()

def init_ext(app):
  db.init_app(app)
  migrate.init_app(app,db)
```

###### 4.manager.py中,增加数据库迁移指令

```python
manager.add_command('db',MigrateCommand)
```

###### 5.执行init命令

```python
>python manager.py db init

>python manager.py runserver -r -d
```

###### 6.mysql因为时区问题无法连接的问题

```python
> mysql -uroot -p123456
> set global time_zone='+8:00';
```

###### 7.连接sqlite的uri

```python
DB_URI = sqlite:///sqlite3.db
```

###### 8.定义模型 models.py

```python
from App.ext import db

class Student(db.model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(16))
```

###### 9.执行数据库迁移

```python
# 1.执行迁移
> python manager.py db migrate

'''
	执行迁移失败的原因,没有任何文件导入model,没有文件知道它的存在
'''
# 在views中导入Student
from .models import Student

# 2.重新执行迁移
> python manager.py db migrate
# 生成表
> python manager.py db upgrade
```

###### 10.db命令

```python
python manager.py db migrate
python manager.py db upgrade

# 撤销操作,表将不存在
python manager.py db downgrade
# 重新将表生成回来
python manager.py db upgrade
```

###### 11.增加数据

```python
@blue.route('/addstudent/')
def add_student():
  student = Student()
  student.name = '小花 %d' % random.randrange(100)
  db.session.add(student)
  db.session.commit()
  return 'Add Success ~~'
```

###### 12.增加数据 add_all() , 列表作为数据源

```python
@blue.route('/addsutdents/')
def add_students():
  students = []
  for i in range(5):
    student = Student()
    student.name = '小绿 %d' % random.randrange(100)
    students.append(student)
  db.session.add_all(students)
  db.session.commit()
  return 'Add Students Success ~'
```

###### 13.查询  query其实是BaseQuery , 在pycharm中 ctrl + n 搜索BaseQuery可以看到这个类中的方法

```python
# 查询结果集
@blue.route('/getstudent/')
def get_student():
  # first获取表中的第一条数据
  student = Student.query.first()
  return student.name
```

###### 14.主键查询

```python
# 查询结果集
@blue.route('/getstudent/')
def get_student():
  # first()返回第一条数据
  # student = Student.query.first()

  # 查找id为3的数据,没有查找到则返回404页面
  # student = Student.query.get_or_404(3)

  # 查找id为4的数据,没有查找到则返回None
  student = Student.query.get(4)
  return student.name
```

###### 15.获取表中的所有数据

######  query.all()

```python
@blue.route('/getstudents/')
def get_students():
  students = Student.query.all()
  for s in students:
    print(s.name)
  return 'get all success~'
```

###### 16.删除

```python
@blue.route('/deletestudent/')
def delete_student():
  # 先查询
  student = Student.query.first()
  # 再删除
  db.session.delete(student)
  db.session.commit()
  return 'delete success~'
```

###### 17.更新数据

```python
# 更新数据
@blue.route('/updatestudent/')
def update_student():
  # 先查询
  student = Student.query.first()
  # 再修改(重新再存一次)
  student.name = 'Tome'
  db.session.add(student)
  db.session.commit()
  return 'update success'
```

###### 18.templates默认会在创建Flask对象的文件的当前目录下的templates目录下寻找, 可以通过template_folder指定其他目录 

```python
# 在App/__init__.py文件中指定templates路径
app = Flask(__name__, template_folder='../templates/')

# 在views.py中注册蓝图的时候指定template_folder
blue = Blueprint('blue', __name__, template_folder='../templates/')
```

###### 19.路由加前缀

```python
# 在创建蓝图的时候指定
blue = Blueprint('blue', __name__, template_folder='../templates', url_prefix='/db')
```

###### 20.静态资源

```html
<!-- static的位置默认也是从创建Flask的文件的同级static目录找的 -->
<link rel="stylesheet" href="/static/css/base.css">

<!-- url-for 反向解析指定css -->
<link rel="stylesheet" href="{{ url_for('static', filename='css/base.css') }}">
```

###### 21.使用flask-debugtoolbar插件

- 使用会报一个错误

```python
# 1.安装
pip install flask-debugtoolbar

# 2.ext.py中注册插件
from flask_debugtoolbar import DebugToolbarExtension			# !!!! 导入
from flask_migrate import Migrate
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
migrate = Migrate()

def init_ext(app):
  db.init_app(app)
  migrate.init_app(app, db)
  DebugToolbarExtension(app) 														# 使用插件
  
# 3.config中需要配置SECRET_KEY否则会报错
SECRET_KEY = 'promise.then.catch'
```

###### 22.根据路径中的id查询

```python
# 查询结果集
@blue.route('/getstudent/<int:id>')
def get_student(id):
  student = Student.query.get(id)
  return student.name
```

###### 23.反向解析

```python
@blue.route('/redirect/')
def redirect():
  # 会将函数解析成路径,并且带参数
  url = url_for('blue.get_student', id=1)
  return url
```

###### 24.约束

```python
primary_key
autoincrement
unique						# 是够唯一
index							# 是否为索引
nullable					# 是否允许为空									

# 以上的约束都为True或者False
default

class User(db.Model):
  # __tasblename 指定表名字
  __tablename = 'uuser'
  id = db.Column(db.Integer, primary_key=True, autoincrement=True)
  u_name = db.Column(db.String(16), unique=True)
  u_des = db.Column(db.String(128), nullable=True)
```

###### 25.Model继承

```python
# 父表
class Animal(db.Model):
  # 必须加__abstract__,否则只会生成一个Animal表,把Dog和Cat的字段都揉在一起
  __abstract__ = True
  id = db.Column(db.Integer, primary_key=True, autoincrement=True)
  a_name = db.Column(db.String(16))

# 子表(会继承父表的字段)
class Dog(Animal):
  d_legs = db.Column(db.Integer, default=4)

# 子表(会继承父表的字段)
class Cat(Animal):
  c_eyes = db.Column(db.Integer, default=2)
  

> python mananger.py db migrate
> python manager.py db upgrade
```

###### 26.数据库连接池 ( django和flask默认都是有数据库连接池的 )

```python
# settings.py中可以配置,默认数据库连接池的数量为5个连接
SQLALCHEMY_POOL_SIZE = 20
```

######  27.条件查询

```python
session.all() 			# 返回列表
其他方式返回的是BaseQuery对象
```

###### 28.filter查询

```python
@blue.route('/getcats')
def get_cats():
  # filter返回的是结果集BaseQuery,页面可以拿到BaseQuery直接循环获取到数据
  cats = Cat.query.filter(Cat.id.__eq__(2))
  print(type(cats))
  return render_template('cats.html', cats=cats)

# .all()会再返回一个 list [] 
cats = Cat.query.filter(Cat.id.__eq__(2)).all()
```

###### 29.普通判断条件也可以使用

```python
__eq__()
__lt__()
__gt__()

# == 也可以使用  >   <   >=   <= 也可以使用
Cat.query.filter(Cat.id == 2)
```

###### 30.其他运算符

```python
query.filter(Cat.a_name.contains('x'))

contains
startswith
endswidth
in_
like
__gt__				# 大于
__ge__				# 大于等于
__lt__				# 小于
__le__				# 小于等于
```

###### 31.其他查询

```python
# 偏移一个,查询两个(即查询第二三条数据), offset和limit不区分顺序
cats = Cat.query.offset(1).limit(2)
cats = Cat.query.limit(2).offset(1)

# order_by查询,这里先使用order_by,为先排序,后通过offset和limit筛选, order_by必须在offset和limit之前
cats =Cat.query.order_by(text('-id')).offset(1).limit(3)
```

###### 32.分页:手动分页

```python
# 1.手动分页
@blue.route('/getdogs/')
def get_dogs():
  page  = request.args.get('page', 1, type=int)                     # 第几页
  per_page = request.args.get('per_page', 4, type=int)              # 每页显示多少条
  dogs = Dog.query.offset(per_page * (page-1)).limit(per_page)      # 查询
  return render_template('dogs.html', dogs=dogs)

# 2.访问的url
http://127.0.0.1:5000/getdogs/?page=2&per_page=4
```

###### 33.分页器分页

```python
# 1.分页器 paginate
@blue.route('/getdogswithpage/')
def get_dogs_with_page():
  # 返回的是分页对象(不可迭代)  .items获取到实际的值
  dogs = Dog.query.paginate().items
  return render_template('dogs.html', dogs=dogs)

# 2.传递数据,仍然和原来一样传递
http://127.0.0.1:5000/getdogswithpage/?page=1&per_page=3
```

###### 34.filter_by

###### filter_by通常用在级联数据查询上

```python
# filter_by(id=5)			条件方式变了,直接 => (属性 = 值)
@blue.route('/getcatsfilterby/')
def get_cats_filter_by():
  cats = Cat.query.filter_by(id=5)
  return render_template('cats.html', cats=cats)
```

###### 35.级联数据 ( 一对多 )

```python
# 级联数据
# 一
class Customer(db.Model):
  id = db.Column(db.Integer, primary_key=True, autoincrement=True)
  c_name = db.Column(db.String(16))

# 多
class Address(db.Model):
  id = db.Column(db.Integer, primary_key=True, autoincrement=True)
  a_position = db.Column(db.String(128))
  # 指定外键
  a_customer_id = db.Column(db.Integer, db.ForeignKey(Customer.id))
  
# 生成迁移文件,生成表
> python manager.py db migrate
> python manager.py db upgrade
```

###### 36.增加级联数据(先增加一的一方,再增加多的一方)

```python
# 增加数据(一的那方)
@blue.route('/addcustomer/')
def add_customer():
  customer = Customer()
  customer.c_name = "剁手党%d" % random.randrange(10000)
  db.session.add(customer)
  db.session.commit()
  return 'customer add success'

# 增加数据(多的那方)
@blue.route('/addaddress/')
def add_address():
  address = Address()
  address.a_position = '广东省河源市龙川县%d' % random.randrange(99999)
  # address对应的id,为数据库中最新的id,即最后一条数据
  address.a_customer_id = Customer.query.order_by(text('-id').first().id)
  db.session.add(address)
  db.session.commit()
  return 'address add success'
```

###### 37.查询级联数据(多查一)

```python
# 通过地址查询用户:多查一
@blue.route('/getcustomer/')
def get_customer():
  a_id = request.args.get('a_id',type=int)
  # 先通过address的id获取到address
  address = Address.query.get_or_404(a_id)
  # 再通过address的a_customer_id 查询到customer
  customer = Customer.query.get(address.a_customer_id)
  return customer.c_name
```

###### 38.一查多(使用filter_by)

```python
# 通过一查多
@blue.route('/getaddresses/')
def get_addresses():
  # 先获取到customer_id,需要由路径参数中传递过来. 再查询到customer
  customer_id = request.args.get('customer_id', type=int)
  customer = Customer.query.get(customer_id)

  # 通过customer id查询到所有的address (使用filter_by方法)
  addresses = Address.query.filter_by(a_customer_id=customer.id)
  return render_template('address.html', addresses=addresses)

# 访问带queryset参数
http://127.0.0.1:5000/getaddresses/?customer_id=2
```

###### 39.自定义addresses属性,可以通过customer获取到对应的address

```python
# 1.修改模型(不需要重新生成数据库迁移文件)
class Customer(db.Model):
  id = db.Column(db.Integer, primary_key=True, autoincrement=True)
  c_name = db.Column(db.String(16))
  # 在这里增加一个address属性,就可以通过customer直接获取到对应的address
  addresses = db.relationship('Address', backref='customer', lazy=True)

class Address(db.Model):
  id = db.Column(db.Integer, primary_key=True, autoincrement=True)
  a_position = db.Column(db.String(128))
  a_customer_id = db.Column(db.Integer, db.ForeignKey(Customer.id))
  
# 2.views中的视图函数修改查询
@blue.route('/getaddresses/')
def get_addresses():
  customer_id = request.args.get('customer_id', type=int)
  customer = Customer.query.get(customer_id)
	# 一查多(customer查询address的时候,直接通过addresses属性获取到对应的address)
  addresses = customer.addresses
  return render_template('address.html', addresses=addresses)
```

###### 40.逻辑运算  与

```python
# 1.链式调用实现 与 运算
@blue.route('/getaddresswithcon/')
def get_address_with_con():
  # 链式调用实现and (查询对应的customer_id为2，且a_position以5结尾的数据)
  addresses = Address.query.filter(Address.a_customer_id.__eq__(2)).filter(Address.a_position.endswith('5'))
  return render_template('address.html', addresses=addresses)

# 2.通过_and实现 与 运算  and_(括号里写两个条件)
@blue.route('/getaddresswithcon/')
def get_address_with_con():
  addresses = Address.query.filter(and_(Address.a_customer_id.__eq__(2),Address.a_position.endswith('5')))
```

###### 41.或运算 or_

```python
@blue.route('/getaddresswithcon/')
def get_address_with_con():

  # 或运算
	addresses = Address.query.filter(or_(Address.a_customer_id.__eq__(2), 			Address.a_position.endswith('5')))	
  return render_template('address.html', addresses=addresses)
```

###### 42.非运算 not_

```python
@blue.route('/getaddresswithcon/')
def get_address_with_con():
  # 非运算
  addresses = Address.query.filter(not_(Address.a_position.endswith('5')))
  return render_template('address.html', addresses=addresses)
```

###### 43.not_运算嵌套or_运算

```python
# not_运算嵌套or_运算
addresses = Address.query.filter(not_(or_(Address.a_position.endswith('5'), 	Address.a_position.endswith('4'))))
```

###### 44.缓存

###### 使用flask-caching ( flask-cache会报错 )

```python
# 1.安装
# pip install flask-cache   这个包运行flask会错误
pip install falsk-caching

# 2.ext.py中注册
from flask_caching import Cache							# !!! 导入缓存
from flask_migrate import Migrate
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
migrate = Migrate()
# 'CACHE_TYPE'指定缓存在哪里,可以写redis
cache = Cache(config={										# 创建一个缓存对象,并指定缓存类型
  'CACHE_TYPE': 'simple',
  'CACHE_TYPE': 'redis'										# 可以写redis,只能写一个
})

def init_ext(app):			
  db.init_app(app)						
  migrate.init_app(app, db)
  cache.init_app(app)											# 初始化
  
# 3.views中使用缓存
from .ext import db, cache

# @cache.cached(timeout=60) 指定缓存时间为60s
@blue.route('/getcats')
@cache.cached(timeout=60)
def get_cats():
  cats =Cat.query.offset(1).limit(3).order_by(text('-id'))
  print(type(cats))
  return render_template('cats.html', cats=cats)

```









