# twbm
JavaScript中有很多奇妙的东西，归咎or归功于设计时候的迅速。缺陷有，但是JavaScript的强大确实体现的淋漓尽致。

它是如此的灵活，当然随之而来的便是开发的代价，它不像强类型语言那样规规矩矩。

一直用着JavaScript，可是有时候有的问题就是很难一时回答得上来，可能大概知道那么些思路，但是又很难回答得清楚，有时候是很需要自己去思考的。难得周末晚上清闲，回味这些看起来有点怪怪却又在发生着的问题。

如果学习需要：[前端教程&开发模块化/规范化/工程化/优化&工具/调试&值得关注的博客/Git&面试-资源汇总][1]

###为什么 [1,2] + [3,4] 不等于 [1,2,3,4]？

 - 原始问题：[stackoverflow question and answer][2]
 - 中文翻译：[高票回答-中文翻译][3]
 - 参考资料：[详解加法运算符][4]

###为什么"0" == !"0" " " == !" " [] == ![] 为true？
 - 原始问题：[知乎提问][5]
 - 参考资料：[英文材料ECMA-262][6] 90页左右的描述，facebook登录后可查看全部
 - 学习参考：[相等运算符和严格相等运算符][7]

###为什么 ++[[]][+[]]+[+[]] = 10？
 - 原始问题：[stackoverflow question and answer][8]
 - 中文翻译：[高票回答-中文翻译][9]
###为什么 javascript 中 0.1 + 0.2 == 0.30000000000000004？

    0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 ==0.9999999999999999
    0.1 * 10 == 1

 
 - 参考资料：[浮点数（从惊讶到思考）][10]
 - 参考资料：[浮点数（谁偷了你的精度？）][11]

###为什么 ["1", "2", "3"].map(parseInt) 返回 [1, NaN, NaN]？

 - 原始问题：[JavaScript Puzzlers!][12]
 - 参考资料：[解析parseInt() 函数][13]
 - 延伸阅读：[你不可能全会的30题-题目][14]
 - 延伸阅读：[你不可能全会的30题-解析][15]
###JavaScript中,{}+{}等于多少?
 - 原始问题：[object-plus-object][16]
 - 中文翻译：[{}+{}等于多少][17]
###JavaScript中,undefined与null的区别？
 - 参考资料：[undefined与null的区别][18]

###为什么 parseInt(0.0000008) === 8？
 - 参考资料：[为什么 parseInt(0.0000008) === 8？中文][19]
###为什么在函数里声明var a = b = 5;在函数外却能访问到b？
 - 参考资料：[写了 10 年 Javascript 未必全了解的连续赋值运算][20]
###call和apply的第一个参数是null/undefined是什么意思？
 - 参考资料：[call和apply的第一个参数为null/undefined时][21]

###querySelectorAll 方法相比 getElementsBy 系列方法有什么区别？
 - 知乎问答：[高票回答][22]
 
随时遇到问题再补充，有好奇心才会有进步！


  [1]: https://segmentfault.com/a/1190000007062464
  [2]: http://stackoverflow.com/questions/7124884/why-does-1-2-3-4-1-23-4-in-javascript
  [3]: http://justjavac.com/javascript/2012/12/18/why-does-1-2-plus-3-4-equal-1-23-4-in-javascript.html
  [4]: https://segmentfault.com/a/1190000007184573
  [5]: https://www.zhihu.com/question/29615998
  [6]: https://zh.scribd.com/document/56770557/ECMA-262
  [7]: http://javascript.ruanyifeng.com/grammar/operator.html#toc6
  [8]: http://stackoverflow.com/questions/7202157/why-does-return-the-string-10
  [9]: http://justjavac.com/javascript/2012/05/24/can-you-explain-why-10.html
  [10]: http://justjavac.com/codepuzzle/2012/11/02/codepuzzle-float-from-surprised-to-ponder.html
  [11]: http://justjavac.com/codepuzzle/2012/11/11/codepuzzle-float-who-stole-your-accuracy.html
  [12]: http://webcache.googleusercontent.com/search?q=cache:http://javascript-puzzlers.herokuapp.com/
  [13]: http://justjavac.com/javascript/2014/02/18/javascript-puzzlers-why-1-2-3-map-parseint-returns-1-NaN-NaN-in-javascript.html
  [14]: https://segmentfault.com/a/1190000006769211
  [15]: https://segmentfault.com/a/1190000006769330
  [16]: http://www.2ality.com/2012/01/object-plus-object.html
  [17]: https://segmentfault.com/a/1190000000264418
  [18]: http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html
  [19]: http://justjavac.com/javascript/2015/01/08/why-parseint-0-00000008-euqal-8-in-js.html
  [20]: http://justjavac.com/javascript/2012/04/05/javascript-continuous-assignment-operator.html
  [21]: http://www.cnblogs.com/snandy/archive/2012/03/01/2373243.html
  [22]: https://www.zhihu.com/question/24702250
