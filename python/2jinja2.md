###### 1jinja2基础

```python
'''
模板中的变量: {{ var }}
	视图传递给模板的数据
	前面定义的数据
	模板中的变量不存在,默认会忽略,不会报错
模板中的标签 {% tag %}
	控制逻辑
	使用外部表达式
	创建变量
	宏定义
'''
```

###### 2.for使用

```python
# 1.路由中传递数据
@blue.route('/students/')
def students():
  student_list = [i for i in range(10)]
  return render_template('Students.html', students=student_list)

# 2.Students.html中渲染视图
<ul>
  {% for student in students %}
    <li>{{ student }}</li>
  {% endfor %}
</ul>
```

###### 3.if使用

```python
{% if False %}
  <span>if为True</span>
{% endif %}

{% if 1==1 %}
  <p>1等于1</p>
{% endif %}
```

###### 4.模板继承

- 相当于slot插槽,父模板留slot给子模板填充
- 模板可以多层继承

```html
<!-- 1.父模板 -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  {% block ext_css %}
  
  {% endblock %}
</head>
<body>
<div>
  {% block header %}
  
  {% endblock %}

  {% block content %}
  
  {% endblock %}

  {% block footer %}

  {% endblock %}

  {% block ext_js %}
  
  {% endblock %}
</div>
</body>
</html>

<!-- 2.子模板,相当于slot插槽,往里面填内容 -->
{% extends 'user/base_user.html' %}
<!-- 默认会覆盖掉header中的内容 -->
{% block header %}
  <h2>这是用户注册的头部</h2>
{% endblock %}

<!-- 使用super()不覆盖内容 -->
{% extends 'user/base_user.html' %}
{% block header %}
	<!-- super()则不会覆盖super中的内容 -->
  {{ super() }}
  <h2>这是用户注册的头部</h2>
{% endblock %}
```

###### 5.include (由零聚一)

```html
<!-- 1.banner.html中 -->
<div>
  <h1>我是一个轮播图!</h1>
</div>

<!-- 2.user_register.html中使用include将banner.html中的内容包含进来 -->
{% extends 'user/base_user.html' %}
{% block header %}
  {% include 'banner.html' %}
{% endblock %}
```

###### 6.宏定义(简单使用)

```python
  {# 宏定义,相当于定义一个函数,返回一个h1标签,然后可以调用这个宏,返回标签 #}
  {% macro hello() %}
    <h3>这是一个macro</h3>
  {% endmacro %}

  {{ hello() }}
  {{ hello() }}
  {{ hello() }}
```

###### 7.宏定义(开发使用)

```html
<!-- 1.专门定义一个html文件放宏定义 html_func.html -->
{# 这个宏定义可传参 #}
{% macro hehe(name) %}
  <h1>哈哈: {{ name }}</h1>
{% endmacro %}

{% macro goods() %}
  <h2>这是一个goods</h2>
{% endmacro %}

<!-- ================================================= -->

<!-- 2.user_register.html中使用上面文件中的宏 -->
{% extends 'user/base_user.html' %}
{% block content %}
	<!-- 可以通过from import 导入别的文件中的宏定义,从而使用它 -->
  {% from 'html_func.html' import hehe %}
  {{ hehe('user_register') }}
  {{ hehe('helllo word') }}
{% endblock %}
```

###### 8.循环器

```python
# 1.视图函数
@blue.route('/getusers/')
def get_users():
  users = ['小白用户%d' % i for i in range(1, 11)]
  return render_template('user/users.html', users=users)

# 2.模板
{% for user in users %}
  <li>{{ loop.index }}:{{ user }}</li>
{% endfor %}

# 3.条件渲染,判断是不是第一个和最后一个
{% for user in users %}
  {% if loop.first %}
    <li style="color: blueviolet">{{ loop.index }}:{{ user }}</li>
  {% elif loop.last%}
    <li style="color: brown">{{ loop.index }}:{{ user }}</li>
  {% else %}
    <li>{{ loop.index }}:{{ user }}</li>
  {% endif %}
{% endfor %}
```

###### 9.过滤器

```python
# flask的过滤器没有限制
{{ 变量 | 过滤器 | 过滤器 }}

capitalize			# 首字母大写
lower						# 小写
upper						# 大写
title				
trim
reverse					# 单词反转
format
stripstags				# 渲染之前,将值中标签去掉
safe
default
last
first
length
sum
sort
```

###### 10.在flask中使用bootstrap

```python
# 1.安装插件(bootstrap3, 使用bootstrap4需要安装bootstrap-flask插件)
pip install flask-bootstrap

# 2.ext.py使用插件
from flask_bootstrap import Bootstrap			# !!! 导入插件
from flask_migrate import Migrate
from flask_session import Session
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
migrate = Migrate()

def init_ext(app):
  db.init_app(app)
  migrate.init_app(app, db)
  Session(app)
  Bootstrap(app)												# 使用插件
  
# 3.创建bootstrap模板 ( 该文件为base.html )
{% extends 'bootstrap/base.html' %}
{% block navbar %}
  <ul class="nav nav-tabs">
    <li class="active"><a href="#">Home</a></li>
    <li><a href="#">SVN</a></li>
    <li><a href="#">iOS</a></li>
    <li><a href="#">VB.Net</a></li>
    <li><a href="#">Java</a></li>
    <li><a href="#">PHP</a></li>
  </ul>
{% endblock %}

{% block content %}
  {% block header %}
  {% endblock %}
  {% block container %}
  {% endblock %}
  {% block footer %}
  {% endblock %}
{% endblock %}

# 4.使用模板 
{% extends 'base.html' %}
{% block navbar %}
  {{ super() }}
{% endblock %}
```



