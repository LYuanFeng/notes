# CSS技巧

### chrome浏览器最小字体大小限制的解决方案

```
chrome有最小字体大小12px的限制，以前可以用-webkit-text-size-adjust：none来解决。但是chrome27以后就不支持了，而且很影响用户体验。我们可以巧用transform来显示小于12px的字体。代码如下：

.font-10px {
     transform: scale(0.5);
     font-size: 20px;
 }

思路很简单，要显示6px的字体，就把字体大小为12px,然后用transform缩小到原来的50%。字体实际显示大小就是6px了。至于transorm的兼容性，这里就不多说了。
```

