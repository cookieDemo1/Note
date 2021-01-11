### scrapy

##### 1.安装

```shell
# 1.安装scrapy
pip install scrapy
# 2.windows上需要安装这个pypiwin32
pip install pypiwin32

# 3.创建项目,然后使用pycharm打开
scrapy startproject myspider

# 创建一个名字为niuza的爬虫，爬取niuza.com的数据
cd myspider
scrapy genspider niuza niuza.com
```

##### 2.配置settings.py中修改

```python
# 这个需要改成False，即不遵爬虫协议
ROBOTSTXT_OBEY = False

# 增加User-Agent伪装请求头
DEFAULT_REQUEST_HEADERS = {
  'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
  'Accept-Language': 'en',
  'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.100 Safari/537.36'
}
```

##### 3.使用爬虫

```python
scrapy crawl niuza
```



