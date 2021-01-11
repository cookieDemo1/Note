##### django2.x  views的使用

- **path** 精准匹配
- **re_path** 正则匹配，和1.x的url一致

```python
# 1.项目urls
from django.contrib import admin
from django.urls import path, include



urlpatterns = [
    path('admin/', admin.site.urls),
    path('views/', include('viewc.urls'))
]

# 2.app中的urls
from django.urls import path, re_path
from . import views
urlpatterns = [
  path('students/', views.students),
  path('student/<int:studentid>', views.student),		# 精准匹配，匹配int
]

# 3.视图函数
from django.http import HttpResponse
from django.shortcuts import render	

# Create your views here.
def students(request):
  return HttpResponse('getStudents')


def student(request, studentid):						# 这里可以接收到匹配的studnetid
  print(studentid)
  return HttpResponse('Get Student Success')
```

##### 2

```python

```

