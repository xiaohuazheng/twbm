## JavaScript富文本编辑器

这篇文章就换个形式写吧，换一换口味。

### contenteditable

U: 我们最近想自己写点儿东西发布，给搞个编辑器呗？

I: 好啊，你在浏览器顶部输入这行代码就可以了：

    data:text/html, <html contenteditable>

U: 别逗，真要一个写富文本的页面，不用花哨，有几个主要功能就行。

I: 了解。

    <div id='editor' contenteditable></div>

I: 看看？

U: 嗯，可以加几个功能按钮嘛？

### document.execCommand

I: 了解。我有这个 ([MDN docs][2])

    bool = document.execCommand(aCommandName, aShowDefaultUI, aValueArgument)

>返回值是一个 Boolean ，如果是 false 则表示操作不被支持或未被启用。

>aCommandName -- 一个 DOMString ，命令的名称。可用命令列表请参阅 命令 。

>aShowDefaultUI -- 一个 Boolean， 是否展示用户界面，一般为 false。Mozilla 没有实现。

>aValueArgument -- 一些命令（例如insertImage）需要额外的参数（insertImage需要提供插入image的url），默认为null。

    <div class="editor-toolbar">
        <a href="javascript: void(0);" data-command='bold'>加粗</a>
        <a href="javascript: void(0);" data-command='insertUnorderedList'>●列表</a>
        <a href="javascript: void(0);" data-command='insertOrderedList'>数字列表</a>
        <a href="javascript: void(0);" data-command='h1'>H1</a>
        <a href="javascript: void(0);" data-command='h3'>H3</a>
        <a href="javascript: void(0);" data-command='p'>P</a>
    </div>    
    $('.editor-toolbar span').click(function(e) {
        var command = $(this).data('command');
        
        if (command == 'h1' || command == 'h3' || command == 'p') {
            document.execCommand('formatBlock', false, command);
        } else {
            var isOk = document.execCommand(command, false, null);
            if (!isOk) {
                // TODO
            }
        }
    });
    
    function getEditorCtnt() {};

I: 瞅瞅？如果不够的话，有很多选择哦：

`backColor`
>修改文档的背景颜色。在styleWithCss模式下，则只影响容器元素的背景颜色。这需要一个<color> 类型的字符串值作为参数传入。注意，IE浏览器用这个设置文字的背景颜色。

`bold`
>开启或关闭选中文字或插入点的粗体字效果。IE浏览器使用 <strong>标签，而不是<b>标签。

`contentReadOnly`
>通过传入一个布尔类型的参数来使能文档内容的可编辑性。(IE浏览器不支持)

`copy`
>拷贝当前选中内容到剪贴板。启用这个功能的条件因浏览器不同而不同，而且不同时期，其启用条件也不尽相同。使用之前请检查浏览器兼容表，以确定是否可用。

`createLink`
>将选中内容创建为一个锚链接。这个命令需要一个HREF URI字符串作为参数值传入。URI必须包含至少一个字符，例如一个空格。（浏览器会创建一个空链接）

`cut`
>剪贴当前选中的文字并复制到剪贴板。启用这个功能的条件因浏览器不同而不同，而且不同时期，其启用条件也不尽相同。使用之前请检查浏览器兼容表，以确定是否可用。

`decreaseFontSize`
>给选中文字加上 <small> 标签，或在选中点插入该标签。(IE浏览器不支持)

`delete`
>删除选中部分.

`enableInlineTableEditing`
>启用或禁用表格行和列插入和删除控件。(IE浏览器不支持)

`enableObjectResizing`
>启用或禁用图像和其他对象的大小可调整大小手柄。(IE浏览器不支持)

`fontName`
>在插入点或者选中文字部分修改字体名称. 需要提供一个字体名称字符串 (例如："Arial")作为参数。

`fontSize`
>在插入点或者选中文字部分修改字体大小. 需要提供一个HTML字体尺寸 (1-7) 作为参数。

`foreColor`
>在插入点或者选中文字部分修改字体颜色. 需要提供一个颜色值字符串作为参数。

`formatBlock`
>添加一个HTML块式标签在包含当前选择的行, 如果已经存在了，更换包含该行的块元素 (在 Firefox中, BLOCKQUOTE 是一个例外 -它将包含任何包含块元素). 需要提供一个标签名称字符串作为参数。几乎所有的块样式标签都可以使用(例如. "H1", "P", "DL", "BLOCKQUOTE"). (IE浏览器仅仅支持标题标签 H1 - H6, ADDRESS, 和 PRE,使用时还必须包含标签分隔符 < >, 例如 "<H1>".)

`forwardDelete`
>删除光标所在位置的字符。 和按下删除键一样。

`heading`
>添加一个标题标签在光标处或者所选文字上。 需要提供标签名称字符串作为参数 (例如. "H1", "H6"). (IE 和 Safari不支持)

`hiliteColor`
>更改选择或插入点的背景颜色。需要一个颜色值字符串作为值参数传递。 UseCSS 必须开启此功能。(IE浏览器不支持)

`increaseFontSize`
>在选择或插入点周围添加一个BIG标签。(IE浏览器不支持)

`indent`
>缩进选择或插入点所在的行， 在 Firefox 中, 如果选择多行，但是这些行存在不同级别的缩进, 只有缩进最少的行被缩进。

`insertBrOnReturn`
>控制当按下Enter键时，是插入 br 标签还是把当前块元素变成两个。(IE浏览器不支持)

`insertHorizontalRule`
>在插入点插入一个水平线（删除选中的部分）

`insertHTML`
>在插入点插入一个HTML字符串（删除选中的部分）。需要一个个HTML字符串作为参数。(IE浏览器不支持)

`insertImage`
>在插入点插入一张图片（删除选中的部分）。需要一个image SRC URI作为参数。这个URI至少包含一个字符。空白字符也可以（IE会创建一个链接其值为null）

`insertOrderedList`
>在插入点或者选中文字上创建一个有序列表

`insertUnorderedList`
>在插入点或者选中文字上创建一个无序列表。

`insertParagraph`
>在选择或当前行周围插入一个段落。(IE会在插入点插入一个段落并删除选中的部分.)

`insertText`
>在光标插入位置插入文本内容或者覆盖所选的文本内容。

`italic`
>在光标插入点开启或关闭斜体字。 (Internet Explorer 使用 EM 标签，而不是 I )

`justifyCenter`
>对光标插入位置或者所选内容进行文字居中。

`justifyFull`
>对光标插入位置或者所选内容进行文本对齐。

`justifyLeft`
>对光标插入位置或者所选内容进行左对齐。

`justifyRight`
>对光标插入位置或者所选内容进行右对齐。

`outdent`
>对光标插入行或者所选行内容减少缩进量。

`paste`
>在光标位置粘贴剪贴板的内容，如果有被选中的内容，会被替换。剪贴板功能必须在 user.js 配置文件中启用。

`redo`
>重做被撤销的操作。

`removeFormat`
>对所选内容去除所有格式

`selectAll`
>选中编辑区里的全部内容。

`strikeThrough`
>在光标插入点开启或关闭删除线。

`subscript`
>在光标插入点开启或关闭下角标。

`superscript`
>在光标插入点开启或关闭上角标。

`underline`
>在光标插入点开启或关闭下划线。

`undo`
>撤销最近执行的命令。

`unlink`
>去除所选的锚链接的<a>标签

`useCSS`
>切换使用 HTML tags 还是 CSS 来生成标记. 要求一个布尔值 true/false 作为参数。注: 这个属性是逻辑上的倒退 (例如. use false to use CSS, true to use HTML).(IE不支持)
该属性已经废弃，使用 styleWithCSS 代替。

`styleWithCSS`
>用这个取代 useCSS 命令。 参数如预期的那样工作, i.e. true modifies/generates 风格的标记属性, false 生成格式化元素。

U: 第一行没有标签包着额，还有不想要div？

I: 哦，那我fix一下，再瞄瞄。

    function getEditorHtml() {
        var str = $('#editor').html();
        if (!str) {
            return '';
        }
    
        str = str.replace(/\<div>/g, '<p>').replace(/\<\/div>/g, '</p>');
    
        if (str[0] != '<') {
            var rIdx = str.indexOf('<');
                str0 = str.substring(0, rIdx),
                pattern = new RegExp(str0);
            
            str = str.replace(pattern, '<p>' + str0 + '</p>');
        }
    
        return str;
    }

### BugFix

U: 我想从其他地方拷贝内容过来，但是怎么有样式?

I: 耶，我处理一下。哈哈哈，偷工一下：

    var eeditor = document.getElementById('editor');
    eeditor.addEventListener('paste', function() {
        setTimeout(function() {
            eeditor.innerHTML = eeditor.innerHTML.replace(/<[^>]*>/g, '');
        }, 0)
    });

U: 光标变了。

I: yeah，这个确实会有这个问题。看来必须去操作 `window.clipboardData` 和 `window.getSelection`啊，这个等等先。一两个浏览器能正常使用就可以吧。

U: 嗯嗯，就我们用用。

I: 那你等我会儿，我看看有啥属性可以粘贴的时候直接粘贴纯文本。

    contenteditable
    contenteditable="true"
    contenteditable="false"
    contenteditable="plaintext-only"
    ontenteditable="events"
    contenteditable="caret"
    

在浏览器都输入框都试下：

    data:text/html, <html contenteditable="plaintext-only">
    data:text/html, <html contenteditable="events">
    data:text/html, <html contenteditable="caret">

发现就 `data:text/html, <html contenteditable="plaintext-only">` 可用。

I: 将就一下，看看呢？

U: 哟，可以用了。

### user-modify

I: yes，那你用着，我去了解下，更多的方式。

翻阅博客，查阅资料，这货： css 属性 `user-modify` 有能力：

    user-modify: read-only;
    user-modify: write-only;
    user-modify: read-write;
    user-modify: read-write-plaintext-only;

当然，只有webkit内核支持的很好： 

    -webkit-user-modify: read-write-plaintext-only

写个demo都试了下，最后这个还挺能满足的。

当然，看了旭哥的[博客][1]后，又学到了许多。所以，不听到IE是多么美好的一天。

### JS paste

引一段张鑫旭的代码：控制粘贴paste事件的JS控制法

    $('#editor').on('paste', function(e) {
        e.preventDefault();
        var text = null;
    
        if(window.clipboardData && clipboardData.setData) {
            // IE
            text = window.clipboardData.getData('text');
        } else {
            text = (e.originalEvent || e).clipboardData.getData('text/plain') || prompt('在这里输入文本');
        }
        if (document.body.createTextRange) {    
            if (document.selection) {
                textRange = document.selection.createRange();
            } else if (window.getSelection) {
                sel = window.getSelection();
                var range = sel.getRangeAt(0);
                
                // 创建临时元素，使得TextRange可以移动到正确的位置
                var tempEl = document.createElement("span");
                tempEl.innerHTML = "&#FEFF;";
                range.deleteContents();
                range.insertNode(tempEl);
                textRange = document.body.createTextRange();
                textRange.moveToElementText(tempEl);
                tempEl.parentNode.removeChild(tempEl);
            }
            textRange.text = text;
            textRange.collapse(false);
            textRange.select();
        } else {
            // Chrome之类浏览器
            document.execCommand("insertText", false, text);
        }
    });

### DEMO

[来个demo][4]

如果你觉得用别人的插件不影响的话，可以试试这个[wangEditor][3]

  [1]: http://www.zhangxinxu.com/wordpress/2016/01/contenteditable-plaintext-only/
  [2]: https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand
  [3]: https://www.kancloud.cn/wangfupeng/wangeditor3/332599
  [4]: https://xiaohuazheng.github.io/demos/2018-03-29-editor-demo.html


