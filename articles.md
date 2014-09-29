文章列表
=========
前端相关
---------
z-index: <http://aijuans.iteye.com/blog/1902780>  
JavaScript Quirks and Flaws：<http://bonsaiden.github.io/JavaScript-Garden/zh/>
<h3>troublesome!</h3>  
>上面的例子中，test 对象从 Bar.prototype 和 Foo.prototype 继承下来；因此， 它能访问 Foo 的原型方法 method。同时，它也能够访问那个定义在原型上的 Foo 实例属性 value。 需要注意的是 new Bar() 不会创造出一个新的 Foo 实例，而是 重复使用它原型上的那个实例；因此，所有的 Bar 实例都会共享相同的 value 属性。

实际上并没有共享value属性，在chrome及node中测试的结果是这样的。说明可能每次在用到prototype的时候都用到了new！  

    function FN() {}
    FN.prototype.method = function(a, b, c) {
        console.info(arguments[0]);
        console.log(a, b, c);
    }
    FN.method = function() {
        Function.call.apply(FN.prototype.method, arguments);
        console.log(arguments[0]);
    };
    var o = new FN();
    o.method(1, 2, 3);
    FN.method(1, 2, 3);

原文中这里有点问题，在<code>Function.call.apply</code>的时候传过去的<code>arguments</code>是正常的，但是在使用<code>FN.method</code>的时候<code>call</code>中的<code>arguments</code>被占用了一个参数，即<code>arguments[0]</code>，使得结果出现问题。

其他
---------
    print 'hello world'