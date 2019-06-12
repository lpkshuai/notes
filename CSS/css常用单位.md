## PX ##
> px像素（Pixel）。相对长度单位。像素px是相对于显示器屏幕分辨率而言的。
> 
> 一般电脑的分辨率有{1024*768}，{1280*768}，{1920*1024}等不同的分辨率
> 
> 1920*1024 前者是屏幕宽度总共有1920个像素宽度后者则是高度为1024个像素

## em ##
> 1.em的值并不是固定的；
> 
> 2. em会继承父级元素的字体大小。
> 
> 任意浏览器的默认字体高都是16px；先看下下面的代码

## rem ##
> rem是css3中新增加的单位相对的只是HTML根元素，默认也是16px；通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应；
> 但是有朋友会问em跟rem有什么区别呢？
> 区别em是根据父元素继承相应大小而不是固定的，rem是继承html根元素的大小，
> 只有改变根元素html的值才能改变rem的值，
> 比如说一个p标签中嵌套一个span标签，p标签字体为28px；span字体为16px；
> 这时候若用em还需要反复计算相应px，而rem就解决了这个繁琐问题；

## 如何使用rem？ ##
> 很多朋友都会说，给html设置一个font-size:62.5%;
> 
> 这样一rem就是10px
> 
> 这样不是不可以，但相应的也会出现一些bug；
> 
> 平时我写移动端写多了也自己封装了一个rem的插件给大家分享出来

![](https://upload-images.jianshu.io/upload_images/11562284-6c3a3600d6e6e78e.jpg?imageMogr2/auto-orient/)

> /* 手机自适应 */(function (doc,win) {var docEl = doc.documentElement,//根元素html        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',         //判断窗口有没有orientationchange这个方法，有就赋值给一个变量，没有就返回resize方法。        recalc = function () {            var clientWidth = docEl.clientWidth;            if (!clientWidth) return;            // if(clientWidth>=560){            //     clientWidth=560;             //把document的fontSize大小设置成跟窗口成一定比例的大小，从而实现响应式效果。            // }            docEl.style.fontSize = 100 * (clientWidth / 750) + 'px';        };    if (!doc.addEventListener) return;    recalc();    win.addEventListener(resizeEvt, recalc, false);    //addEventListener事件方法接受三个参数：    //第一个是事件名称比如点击事件onclick，第二个是要执行的函数，第三个是布尔    doc.addEventListener('DOMContentLoaded', recalc, false);//绑定浏览器缩放与加载时间})(document, window);

> 创建一个js将上面代码粘贴到其中，在页面中引入此js，然后什么都不用设置了，此时此了1rem等于100px；这样就很方便及算了；比如一般ios设计比例为750*1334；我们可以设置width:7.5rem;这样宽度就是750px；然后字体大小呢，如设计稿上标明28px，则可以设置font-size:0.28rem,随着你的手机不通宽度不同，他会相对的改变你的html根元素的大小，这时也就形成了响应式移动端，手机比例越大宽度越大，字体越大，这样就将一张页面实现不同的宽高与大小。

## %（百分比） ##

> %也很常见，它和em差不多一样，都是**相对于父级元素**。但%可以在很多属性中使用，比如：width、height、font-size等。而em是用来设置字体大小（font-size）的单位，width、height等属性是没有em单位的。

**一般来说：1em = 1rem = 100% = 16 px**