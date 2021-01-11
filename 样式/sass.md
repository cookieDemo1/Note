## less

###### 1.安装

```bash
# 1.安装
$ npm install -g less
```

###### 2.命令行用法

```bash
# 普通转换方法
$ lessc style.less style.css

# 需要安装clean-css插件,可以缩小输出内容
$ lessc --clean-css style.less style.min.css
```



###### less注释

```less
// 该注释不会编译到css文件中
/* 该注释会编译到css文件中 */		
```

## sass

###### 1.安装sass

```bash
# 1下载安装ruby

# 2.安装sass
gem install sass

# 3.安装compass即可,因为compass会依赖sass,所以会自动安装sass
gem install compass

# 查看安装源
gem sources
```

###### 2.创建工程(compass的方式)

- sass的方式只要求创建一个文件夹即可

```bash
$ compass create my-sass

# 目录解析
sass						# sass文件夹
stylesheets					# 生成的css文件
config.rb					# 
```

### sass基础

###### 1.编译命令

```bash
# 没有指定目录,默认生成在当目录下
$ sass demo1.scss demo1.css

# 监听scss文件的改变,当文件改变后,会自动编译.对应的css文件也会编译(该命令会阻塞在cmd)
$ sass --watch demo1.scss:demo1.css

# 监听文件夹所有的scss文件的改变,该文件夹下的sass文件改变后,会自动编译
$ sass --watch sass:stylesheets

# 在项目根路径下,执行此命令,会自动编译sass文件夹下的所有文件(不需要加路径参数)
$ compass compile

# compass watch监听文件夹的改变,里面的scss文件改变后自动编译
$ compass watch

# 监视并指定风格(compact为一个选择器一行的风格)
$compass watch --force -s cpmpact
```

###### 2.注释

```scss
// 单行注释
/* 单行标准css注释 */
```

###### 3.sass变量

```scss
// 一 局部变量
body{
  // 1.局部变量,有作用域,在当前块里面,即当前{}里面
  $color: red;
  // 2.使用变量
  color: $color;
}


// 二 全局变量
body{
  // 1.全局变量
  $color: red !global;
}

footer{
  // 2.使用全局变量
  color: $color
}

// 变量默认值
$font-size: 14px !default;

// 定义多值变量
$paddings: 5px 10px 5px 10px;
body{
  // 使用多值变量
  padding: $paddings;
  // 取出多值变量的第一个值(下标从1开始)
  padding-left: nth($paddings, 1);
}

// maps类型的变量
$maps: (color: red, border-color: blue);
body{
  // 使用map中的值
  background-color: map-get($maps, color);
}


// 变量的他特殊用法,可以用在选择器上
$className: container;
.#{$className}{
  width: 50px;
  height: 50px;
}

// 不区分下划线和中划线,它们是同一个变量
$color-info: green;
$color_info: green;
body{
  color: $color-info
}
```

###### 4.部分文件

```scss
// 以下划线开头的文件就是部分文件,部分文件只用来被导入的,不会编译成css文件
_part1.scss

// 导入部分文件,直接写part1即可,不用加下划线和后缀(在当前目录下)
@import "part1";
```

###### 5.嵌套的写法

```scss
// sass嵌套语法
body{
  header{
    .log{
      width: 100px;
      display: inline-block;
    }
    .item{
      width: 20px;
    }
  }
}
```

###### 6嵌套

```scss

```

###### 7继承

```

```

