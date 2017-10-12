## 业务代码中可用的CSS技巧

### 兼容chrome下的10px字体

    p {
        font-size: 10px;
        -webkit-transform: scale(.83);
    }

此方法在前端页面需要展示更小字体，兼容浏览器时非常有用。

### 文本溢出显示省略号...

    p {
        display:block;
        white-space:nowrap; 
        overflow:hidden; 
        text-overflow:ellipsis;
    }

此方法对前端页面容错非常有效，因为一个元素里的内容太多，就会导致显示不完，省略号让用户体验更好。如果元素宽高没限制，内容太多会导致文本溢出，严重影响用户体验。

### 给元素增加hover渐变效果

    div {
        width:200px;
        height:200px;
        transition: box-shadow .5s;
        -moz-transition: box-shadow .5s;
        -webkit-transition: box-shadow .5s;
        -o-transition: box-shadow .5s;
    }
    div:hover {
        box-shadow: 0 14px 20px #666;
    }

鼠标经过会有阴影效果，鼠标离开恢复。

### 居中div

常用margin方法：

    div { 
        width:200px;
        margin:0 auto;
    }

1/2宽高的margin，50%的left、top方法：

    div {        
        Width:500px ; 
        height:300px;      
        Margin: -150px 0 0 -250px;        
        position:relative;       
        background-color:pink;      
        left:50%;
        top:50%;     
    }

LTRB值为0的方法：

    div { 
        width: 400px;
        height: 300px; 
        margin:auto;
        position: absolute; 
        left: 0; 
        top: 0; 
        right: 0; 
        bottom: 0;
    }

transform方法

    div {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

带文本元素的话，外面包一层div，让里面的line-height = height：

    p {
        height:30px;
        line-height:30px;
    }

flex弹性盒子布局居中：

    div {
        display: flex;
        flex-flow: row wrap;
        width: 408px; 
        align-items: center; 
        justify-content: center;
    } 

### 默认继承

    默认继承： font-size font-family color等, 元素有UL LI DL DD DT等;    
    不可继承： border padding margin width height 等表现元素大小的属性。

### 清除浮动

结尾处使用div空标签清除浮动(非div需要display:block)，不过这种方式向DOM添加了不必要的元素，使用不是很多。

    .clear{clear:both;} 

利用overflow属性。必须定义width或zoom:1，同时不能定义height，应用值为hidden或auto的overflow属性有一个有用的副作用，这会自动清理包含的任何浮动元素。

    div {
        float:none;
        overflow:hidden; 
    } 

给父级div定义伪类：after和zoom(zoom为IE6.7专属)

    .clearfix:after{
        content:'';
        height:0;
        display:block;
        visibility:hidden;
        clear:both
    }

### 最大最小宽度高度兼容

当使用height:auto 时应考虑到元素最大或最小的宽度高度，以便容错。、

    div {
        min-height:500px;
        height:auto;
        max-width:500px;
        width:auto;
    }

### cursor 属性（禁点）

    <a href="mailto:xzavierhua@gmail.com" "email me">email me</a>
    a {cursor:not-allowed;}  

   cursor其他常用属性

    default 默认光标（通常是一个箭头）
    auto       默认。浏览器设置的光标。
    crosshair  光标呈现为十字线。
    pointer 光标呈现为指示链接的指针（一只手）
    move       此光标指示某对象可被移动。
    text       此光标指示文本。
    wait       此光标指示程序正忙（通常是一只表或沙漏）。
    help       此光标指示可用的帮助（通常是一个问号或一个气球）。

### window font && mac font 兼容

    * {
        font-family: "PingFang SC","microsoft yahei",Arial,Helvetica,sans-serif;
    }
'
PingFang SC'是mac下和微软雅黑接近的字体，'microsoft yahei'同'微软雅黑'，不过有些网站不兼容中文(GB2312)的字符还是用英文的比较好，Arial主要用于文字中的数字。

### 阻止事件

    <a href="mailto:xzavierhua@gmail.com" "email me">email me</a>
    a {pointer-events:none;}

### 禁止选中文本

    * {
        -moz-user-select:none;
        -webkit-user-select:none;
        -ms-user-select:none;
        user-select:none;
    }

### a标签hover

被点击访问过的超链接样式不在具有hover和active的样式了,解决方法是改变CSS属性的排列顺序: 
`L-V-H-A（link,visited,hover,active）`

### CSS写一个简单的幻灯片效果

    div {
          width:480px;
          height:320px;
          margin:50px auto;
          overflow: hidden;
          box-shadow:0 0 5px rgba(0,0,0,1);
          background-size: cover;
          background-position: center;
          -webkit-animation-name: "loops";
          -webkit-animation-duration: 20s;
          -webkit-animation-iteration-count: infinite;
    }
    @-webkit-keyframes "loops" {
        0% {background:url(0.jpg) no-repeat;}
        25% {background:url(1.jpg) no-repeat;}
        50% {background:url(2.jpg) no-repeat;}
        75% {background:url(3.jpg) no-repeat;}
        100% {background:url(4.jpg) no-repeat;}
    }

### rgba()和opacity

rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度 ; 

    div {
        opacity:0.5;
    }

而rgba()只作用于元素的颜色或其背景色。设置rgba透明的元素的子元素不会继承透明效果。

    div {
        background: rgba(255,255,0,0.8);
      }

### 使用圆角

    div {  
        -moz-border-radius: 5px;  
        -webkit-border-radius: 5px;  
        border-radius: 5px;  
    }

### !important

css优先级为:   !important > 内联样式 >  id > class > tag      
所以，使用!important时需要小心。

### 伪类使用

    p:first-of-type 选择属于其父元素的首个p元素。     
    p:last-of-type  选择属于其父元素的最后p元素。     
    p:only-of-type  选择属于其父元素唯一的p元素。     
    p:only-child    选择属于其父元素的唯一子元素。     
    p:nth-child(n)  选择属于其父元素的第n个子元素。     
    :enabled :disabled 控制表单控件的禁用状态。     
    :checked 单选框或复选框被选中
    ::selection  匹配被用户选取的选取是部分

### position的relative和absolute

position参考文章：[详解css相对定位和绝对定位][1]

    <div class="parent">
        <div class="child"></div>
    </div>
   
    .parent{
        position: absolute;  //变换这段代码做测试
        border: 1px solid #ccc;
        height: 200px;
        width: 200px;
    }
    .child{
        position: relative;  //变换这段代码做测试
        left: 100px;
        top:100px;
        background: blue;
        height: 50px;
        width: 50px;
    }

relative：

    如果设定TRBL，并且父级没有设定position属性，仍旧以父级的左上角为原点进行定位。
    如果设定TRBL，并且父级设定position属性，则以父级的左上角为原点进行定位，位置由TRBL决定。如果父级有Padding属性，那么就以内容区域的左上角为原点，进行定位。
    相对定位总是以父级左上角为原点进行定位的，如果父级不存在，则以浏览器左上角进行定位。

    
absolute：

    如果设定TRBL，并且父级没有设定position属性，那么当前的absolute则以浏览器左上角为原始点进行定位，位置将由TRBL决定。    
    如果设定TRBL，并且父级设定position属性，则以父级的左上角为原点进行定位，位置由TRBL决 定。只以父级左上角为原点进行定位，父级的padding对其根本没有影响。    
    使用绝对定位的盒子以它的“最近”的一个“已经定位”的“祖先元素”为基准进行定位。如果没有已经定位的祖先元素，那么会以浏览器窗口为基准进行定位。
    绝对定位的框从标准流中脱离，这意味着他们对其后的兄弟盒子的定位没有影响，其他的盒子好像就好像这个盒子不存在一样。
    

### 使用新特性

    盒子阴影（box-shadow）
    文本阴影（text-shadow、）    
    transform:rotate(9deg)scale(0.85,0.90)translate(0px,-30px)skew(-9deg,0deg);// 旋转,缩放,定位,倾斜
    媒体查询 @media (min-width: 1280px)
    border-image 嵌入图片形式的边框 border-image: url(border.png) 27/27px round;
    线性渐变（gradient） linear-gradient 线性渐变  radial-gradient 径向渐变 等等


  [1]: https://segmentfault.com/a/1190000000680773