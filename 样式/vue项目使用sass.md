###### 1.安装插件

```bash
npm install node-sass -save-dev
npm install sass-loader --save-dev
```

###### 2.在.vue文件中使用sass

```html
<style scoped lang="scss" >
	$color: red;					/* 定义变量 */
    h1,h2{
        color: $color;				 /* 使用变量 */
    }
</style>
```



