### asyns.waterfall(tasks,callback) 使用说明
#### 参数
- tasks  
是一个函数数组
- callback
最终的回调函数

#### 代码举例
```javascript
async.waterfall([function(cb) {
        cb(null, 'hello');
    }, function(param1, cb) {
        cb(null, 'hello', 'world');
    }, function(param1, param2, cb){
        cb(null, 'result');
    }], function(err, result) {
        console.log(result);
});
```
#### 特殊要求
- tasks中,每个函数需要有一个函数参数cb
- tasks中,前一个函数需要按照后一个函数的参数表调用cb,并且第一个参数必须为null,否则会跳过后续的task直接调用callback,