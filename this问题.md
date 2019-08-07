### 1.1
```
function show () {
    console.log('this: ', this);
}
var obj = {
    show: show
}
obj.show();


var obj = {
    show: function () {
        show()
    }
}
obj.show()

// output: obj      window
```
解析： 第一个obj.show(),输出obj,这是因为被某对象通过属性访问器调用的函数，被谁调用的this就指向谁。  第二个obj.show()输出window，这是因为这个show函数内部是通过show()方法自执行，进行打印的，对于直接调用的函数，不论他在哪里执行，他的this都指向window

### 1.2
```
var obj = {
    show: function () {
        console.log('this: ', this);
    }
};

(0, obj.show)()

// output: window
```
解析: 首先要先说一下逗号表达式的用法，逗号表达式总是返回最后一项，也就是说上一步逗号表达式返回的是obj对象中的show方法，然后在进行函数的自执行，同理，对于直接调用的函数，不论他在哪执行，this都指向window

### 1.3
```
var obj = {
    show: function() {
        console.log('this: ', this);
    }
};
var newObj = new obj.show();

// output: newObj
```
解析: 当调用new的时候会创建一个新对象，并且将构造函数的this指向所创建的新对象，所以这里的this是指向new所创建的新的对象也就是newObj

### 1.4
```
    var obj = {
        show: function() {
            console.log('this: ', this);
        }
    };

    var newobj = new (obj.show.bind(obj))()
```
解析: 这里有一个this绑定优先级的问题new绑定 > 显示绑定 > 隐式绑定,所以这段代码中new操作符会替换掉bind绑定的this,将this指向new所创建的新的对象也就是newobj

### 1.5
```
var obj = {
    show: function() {
        console.log('this: ', this);
    }
};

var elem = document.getElementById('allways study');
elem.addEventListener('click', obj.show);
elem.addEventListener('click', obj.show.bind(obj));
elem.addEventListener('click', function() {
    obj.show();
});

// output:  1. elem   2.obj   3.obj
```
解析: 第一个输出是因为addEventListener回调函数执行的时候this是指向调用该回调的元素的，第二个输出是因为，点击的回调函数是通过bind修改过的函数，这个函数的this是指向obj的，第三个就是前面说到的这是因为被某对象通过属性访问器调用的函数，被谁调用的this就指向谁，所以这是this指向的是调用他的obj。