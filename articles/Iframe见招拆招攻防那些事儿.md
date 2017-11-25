# iframe - 见招拆招

iframe - 防止被嵌套，防止别人阻止自己的页面被嵌套，一攻一防，功守道

## 第一招 - 打拳 - 使用别人的页面

    <iframe src="https://www.google.com"></iframe>

当然，这是最简单的招了，也阔以说是写笔记为了凑招，哈哈，反正iframe一下就可以套用别人的页面了。
你所了解的什么攻击啊，壳子啊，说不定就是这个东西干的。

把别人的页面iframe嵌套在自己的页面，然后可以控制元素，比如说换个logo啊，盗取下输入啊什么的，靠，真的可以骗人。

但是...我不做这些，我用iframe是因为业务需求，我们嵌套别人的页面是为了做网页双屏，更快的帮别的网上审核内容。

## 第二招 - 拆招 - 防止被iframe

你想套路我？那不好意思了，你当前的url也会变成我的，哈哈哈哈哈哈哈...

禁止你的网页被iframe的方法的原理简单：判断你的网页的location和顶级窗口的location是否相同。
如果不相同，则说明你的网页被iframe套了，还不去拯救。

各种写法随意发挥：

    var url=window.location.href;
    if (window != parent) {
        parent.navigate(url);
    }  


    if (window.frames.length != parent.frames.length) {
        window.parent.location.href = self.location.href;
    }


    if (top != self) {
        top.location.href = self.location.href;
    }

转载下别人的代码（前面的代表使用率）：

判断是否被iframe：

    38%   if (top != self)
    22.5% if (top.location != self.location)
    13.5% if (top.location != location)
    8%    if (parent.frames.length > 0)
    5.5%  if (window != top)
    5.5%  if (window.top !== window.self)
    2%    if (window.self != window.top)
    2%    if (parent && parent != window)
    2%    if (parent && parent.frames && parent.frames.length>0)
    2%    if((self.parent&&!(self.parent===self))&&(self.parent.frames.length!=0))

防止被iframe代码：

    7 top.location = self.location
    4 top.location.href = document.location.href
    3 top.location.href = self.location.href
    3 top.location.replace(self.location)
    2 top.location.href = window.location.href
    2 top.location.replace(document.location)
    2 top.location.href = window.location.href
    2 top.location.href = "URL"
    2 document.write(’’)
    2 top.location = location
    2 top.location.replace(document.location)
    2 top.location.replace(’URL’)
    1 top.location.href = document.location
    1 top.location.replace(window.location.href)
    1 top.location.href = location.href
    1 self.parent.location = document.location
    1 parent.location.href = self.document.location
    1 top.location.href = self.location
    1 top.location = window.location
    1 top.location.replace(window.location.pathname)
    1 window.top.location = window.self.location
    1 setTimeout(function(){document.body.innerHTML=’’;},1);
    1 window.self.onload = function(evt){document.body.innerHTML=’’;}
    1 var url = window.location.href; top.location.replace(url)

各种手段尽在[pdf文件][1]

## 第三招 - 再拆 - 防止iframe页面自动跳转

所谓道高一尺魔高一丈, 比武招亲，噢，是擂台比武，见招拆招，邪不胜正，邪亦正，正亦邪，乾坤大挪移。回来，回来。
逼急了，一练七伤，七者皆伤，禁用你的JS，看你跳! 

但是，能不伤自己的，我们还是悠着点儿，拿好武器：

### HTML5 的sandbox属性，

HTML的iframesandbox属性, 是启用一系列对 <iframe> 中内容的额外限制。
值： 

    "" / allow-forms / allow-same-origin / allow-scripts / allow-top-navigation

不设置就都不取消，设置""全部取消，后面的是挨个取消。

显然，我们要做的不是取消allow-scripts，而是allow-top-navigation。

    <iframe sandbox="allow-same-origin allow-scripts allow-popups allow-forms allow-pointer-lock" src="http://www.google.com"</iframe>

IE 有个 security="restricted" 来做限制，是禁止掉js，显然不符合预期，而且在Chrome会反复跳转循环，IE嘛...

其实我做的时候这个就解决了，但是我看还有网上的方式，也试了下，但都不如第一个。

1、双重iframe可以阻止强制跳转。也就是说，你用页面A iframe一个自己的页面B，在B里再iframe你真正想iframe的页面C。问题在于，第一层的iframe就覆盖了第二层的，所以要把第一层的做成透明的。

2、window.onbeforeunload页面卸载事件判断

    var prevent_bust = 0;
    window.onbeforeunload = function() { 
        prevent_bust++ 
    }
    setInterval(function() {
        if (prevent_bust > 0) {
            prevent_bust -= 2;
            window.top.location = 'http://server-which-responds-with-204.com';
        }
    }, 1);

这也有问题啊，A iframe了B, B改变top.locations时会触发window.onbeforeunload事件，A捕获到后，location到这个“204页面”。但此时A页面的链接都失效了，可以改下prevent_bust -= 3，就可以免去这个问题。然而，还是有问题啊，你必须保障别人的iframe会按你如期的放被iframe，但我要iframe万万个页面，显然，pass.

不过，还是可以研究的：[研究地址][2]

所以，其实有一招就不错了，说不定，还有第四招。

## 第四招 - 再挡 - 防止别人阻止自己的防被iframe代码执行

好，逻辑还清晰，但是，我还不知道，反正我目的达到了，而且，再做下去，就是个死循环了，都是程序猿，何苦呢，而且，我没做坏事儿。


  [1]: http://seclab.stanford.edu/websec/framebusting/framebust.pdf
  [2]: https://blog.codinghorror.com/we-done-been-framed/

