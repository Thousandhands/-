### 一
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