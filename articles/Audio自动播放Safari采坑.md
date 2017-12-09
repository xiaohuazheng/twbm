## 背景

我在哪些时候用到这个标签呢？

- 1、学习实践得时候，拿着Chrome的调试器，一番乱怼，用着还挺有意思的。但没有怎么深入研究兼容性之类的，只是了解低阶浏览器不兼容，需要hack。
- 2、后台系统需要记录并展示语言内容，后台嘛，简单实在，直接扔了个 `audio` ，无需担心浏览器和场景，业务要紧。
- 3、前端需要做个贺卡页面，用户点击进入贺卡页面自动播放音乐。这个也不需要做特别的样式，就一个音乐图标，点击暂定，点击继续播放。（难点在于有的手机浏览器就是不给你自动播放-safari,wechat）
- 4、前端需要展示音频，并不要后台那种官方美颜，需要独立特行些，带个进度条，时间什么的就最好了。OK，没问题，写样式嘛，谁该不会个渐变啊动画啊什么的。
- 5、前端需要展示用户语音内容并可以播放，并做好看的样式，像微信那样的展示时间等。OK，没问题，给我个音频链接就行，其他放着我来。（难点在于有的浏览器就是不给支持一些事件）

什么难点啊，这是做这行的乐趣...

## 实现

实现，也没啥，扔个标签：

```
<audio id="audio" src="http://www.xzavier.mp3">
您的浏览器不支持 audio 标签。
</audio>
```

PM说：开发，你辛苦了，不要去管那些不支持的浏览器，把中间那句话砍掉，扑通一声，我差点跪下。

但其实，我还是写了兼容了，先注释在那儿，说不定...

    <embed src="http://www.xzavier.mp3" width="0" height="0" volume="100" autostart="true" loop="true" />

再配上一个 `Stop()` 和一个 `Play()` 应该就够用了。

那不兼容就不兼容咯，继续写。

    <audio id="audio" src="http://www.xzavier.mp3"></audio>

属性什么的，挨个试一遍，能用用，这几个好像挺频繁的使用：

    属性             值                描述
    autoplay    autoplay    如果出现该属性，则音频在就绪后马上播放。
    controls    controls    如果出现该属性，则向用户显示控件，比如播放按钮。
    loop        loop        如果出现该属性，则每当音频结束时重新开始播放。
    src         url         要播放的音频的 URL。
    preload     preload     如果出现该属性，则音频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。（auto - 当页面加载后载入整个音频；meta - 当页面加载后只载入元数据；none - 当页面加载后不载入音频）

更多：

    属性                 描述
    audioTracks         返回表示可用音频轨道的 AudioTrackList 对象。
    autoplay            设置或返回是否在就绪（加载完成）后随即播放音频。
    buffered            返回表示音频已缓冲部分的 TimeRanges 对象。
    controller          返回表示音频当前媒体控制器的 MediaController 对象。
    controls            设置或返回音频是否应该显示控件（比如播放/暂停等）。
    crossOrigin         设置或返回音频的 CORS 设置。
    currentSrc          返回当前音频的 URL。
    currentTime         设置或返回音频中的当前播放位置（以秒计）。
    defaultMuted        设置或返回音频默认是否静音。
    defaultPlaybackRate 设置或返回音频的默认播放速度。
    duration            返回音频的长度（以秒计）。
    ended               返回音频的播放是否已结束。
    error               返回表示音频错误状态的 MediaError 对象。
    loop                设置或返回音频是否应在结束时再次播放。
    mediaGroup          设置或返回音频所属媒介组合的名称。
    muted               设置或返回是否关闭声音。
    networkState        返回音频的当前网络状态。
    paused              设置或返回音频是否暂停。
    playbackRate        设置或返回音频播放的速度。
    played              返回表示音频已播放部分的 TimeRanges 对象。
    preload             设置或返回音频的 preload 属性的值。
    readyState          返回音频当前的就绪状态。
    seekable            返回表示音频可寻址部分的 TimeRanges 对象。
    seeking             返回用户当前是否正在音频中进行查找。
    src                 设置或返回音频的 src 属性的值。
    textTracks          返回表示可用文本轨道的 TextTrackList 对象。
    volume              设置或返回音频的音量。

## 加效果

那就把原来的标签隐藏了，自己写标签，通过api去控制播放等。

    audio {
        width: 0;
        height: 0;
        visibility: hidden;
    }

然后自己写控件，自己写样式，然后我们用js去控制音频，爽歪歪。

    play：开始播放
    pause：暂停播放

更多方法：

    方法             描述
    addTextTrack()  向音频添加新的文本轨道。
    canPlayType()   检查浏览器是否能够播放指定的音频类型。
    fastSeek()      在音频播放器中指定播放时间。
    getStartDate()  返回新的 Date 对象，表示当前时间线偏移量。
    load()          重新加载音频元素。
    play()          开始播放音频。
    pause()         暂停当前播放的音频。


## 解决问题

### IOS自动播放

可以用了。自动播放？有个 `autoplay` 啊。

不过safari可没答应，微信好像当时也没答应，但后来答应了。
大概意思就是ios觉得自动播放这种方式暴力不友好，所以把你禁了，需要用户有交互行为才给开门，即Safari屏蔽了autoplay，必须由用户交互事件触发。要给PM解释，可以给他看看这个 [developer][1]，也阔以读懂了自己吹。

Safari假装自动播放，就用 `touchstart` ，摸一下，解决了。

    document.addEventListener('touchstart', function(){ 
        audio.play();
    }, false);

如上，Safari对这俩事件再没有用户交互行为前也是支持的，但还是不能播放。所以暂时就用上面的假装一下自动播放了。

    audio.addEventListener('progress', function() {
        audio.play();
    });
    
    audio.addEventListener('loadedmetadata', function() {
        audio.play();
    });

那么 `touchstart` 就是暂时的解决方案了。Stack Overflow上的人说：

`这是苹果的一个商业决定，并不是一个技术决定。` 哈哈，哈哈哈，还是挺有意义的。


### 其他事件

你也阔以都试下audio的事件：

    loadstart ：浏览器开始请求媒介；
    durationchange ：媒介时长（duration属性）改变；
    loadedmetadata ：浏览器获取完媒介资源的时长和尺寸
    loadeddata ：已加载当前播放位置的媒介数据；
    progress ：浏览器正在获取媒介；
    canplay ：浏览器能够开始媒介播放，但估计以当前速率播放不能直接将媒介播放完（播放期间需要缓冲）；
    canplaythrough ：浏览器估计以当前速率直接播放可以直接播放完整个媒介资源（期间不需要缓冲）；
    timeupdate ：当前播放位置（currentTime属性）改变；
    ended ：播放由于媒介结束而停止；
    ratechange：默认播放速率（defaultPlaybackRate属性）改变或播放速率（playbackRate属性）改变；
    volumechange ：音量（volume属性）改变或静音（muted属性）。

`loadstart，durationchange，loadeddata，progress，canplay，canplaythrough`顺序发生。

### 时间问题

我用loadeddata去监听，然后执行播放和显示时间：

    audio.addEventListener('loadeddata', function() {
        console.log(Math.ceil(audio.duration));
        audio.play();  // 普通机子也阔以用这个监听后播放
    });

然后非ios设备解决了，ios还是需要用户有交互行为才能执行上面的回调，啊哈，那这些 `/iPad|iPhone|iPod/` 和Android表现不一，没有显示时间咋办。

还是如上，用IOS支持的事件就可以了，ios允许 `progress | loadedmetadata` 等，其他的你自己动手试试：

    audio.addEventListener('progress', function() {
        console.log(Math.ceil(_this.voice[vid].duration));
    });

yeah，这样ios也阔以了，里面的逻辑判断就自己去写了，阔以判断 `navigator.userAgent` ，也阔以判断是否已经执行过其中一个回调了。

### 微信

微信的话，不管，肯定不行，流量啊。

可以加载这个js : `http://res.wx.qq.com/open/js/jweixin-1.0.0.js`

然后无需申请appid什么的，参数也阔以不填。

    //配置微信信息  
    wx.config ({  
        debug : false,    // true:调试时候弹窗  
        appId : '',  // 微信appid  
        timestamp : '', // 时间戳  
        nonceStr : '',  // 随机字符串  
        signature : '', // 签名  
        jsApiList : []  // 所有要调用的 API 都要加到这个列表中   
    });  
    wx.ready (function () {  
        var audio = document.createElementt('audio');
        audio.src = 'http://www.xzavier.mp3';
        audio.addEventListener('canplay', function() {
            audio.play();
        });
    }); 

## More

问题都解决了，那其实也没啥难得了。

进度条？我想了下，就一个背景色，一个前景色覆盖就好了，然后根据播放时间调前景色的百分比就OK了。

微信个人语音播放效果？我想了下，阔以用js写个定时器去动画嘛，也阔以用gif嘛，也阔以用CSS3的animation嘛，控制一下class就好。


以上，应该是基于你不想用某些组件，或者别人的代码太多不想引入，想自己动手的解决方案。希望有助！


  [1]: https://developer.apple.com/library/content/documentation/AudioVideo/Conceptual/Using_HTML5_Audio_Video/Device-SpecificConsiderations/Device-SpecificConsiderations.html


