###### 1.替换元素的object-fit和object-position

```css
img#mine-img2{
    width: 500px;
    height: 500px;
    border: 1px solid #000;
    /* object-fit属性,指定可替换元素的内容应该如何适应到期使用的高度和宽度确定的框 */
    object-fit: none;
    /* object-position属性，规定了可替换内容，在其内容框中的位置 */
    object-position: left top;
}
```

###### 2.图片的content

```css
.img1{
    width: 100px;
    height: 100px;
}
/* 如果图片原本是有src地址的，我们可以使用content属性把图片内容替换掉，于是就能轻松实现hover图片变成另外一张图片的效果，background-image也可以实现。content属性改变的仅仅是视觉的呈现，当我们右键或其他形式保存图片的时候，所保存的还是原来src对应的图片 */
.img1:hover{
    content: url(b.PNG);
}
```

###### 3.content属性让普通标签元素变成替换元素

```css
/* 在CSS世界中，我们把content属性生成的对象成为“匿名替换元素” */
h1{
    content: url(b.PNG);
}

/* 在实际项目中，content属性大都是用在::before和::after这两个伪元素中，因此content内容生成计数，有时候也被成为::before和::after伪元素计数 */
```

