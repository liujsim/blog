    
    
### CSS 绘画
CSS 绘画基础

border 默认是黑色，可以通过设置为 transparent 隐藏

预备知识：`border-left-color`、 `border-radius` 、`box-shadow`、`border-bottom-right-radius`


html

    <div class="proto clearfix"></div>

css

    .proto{
     float: left;
     width: 100px;
     height: 100px;
     background-color: aquamarine;
     margin: 20px;
     border: 30px solid;
     border-left-color: blueviolet;
     border-bottom-color: brown;
     border-right-color: yellowgreen;
     border-top-color: teal;
    }
    
    
![](http://swordshadow.qiniudn.com/2016-05-19-14636572952916.jpg)

通过设置 `border-left-color` 等属性可以实现三角形，梯形等效果，更多效果参考 [CSS shape](http://codepen.io/runcelim/pen/MyMeMv)


### CSS 复杂图形和动画应用

[示例demo](http://codepen.io/runcelim/pen/QNXKLG)


### CSS 绘图的应用



在开发中常用来做各种 loader [CSS Spinners](http://projects.lukehaas.me/css-loaders/)

[How TO - CSS Loader](http://www.w3schools.com/howto/howto_css_loader.asp)

[codepen Loaders and Spinners](https://codepen.io/collection/HtAne/)
     

