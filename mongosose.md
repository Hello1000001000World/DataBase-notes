# mongoose

## 安装

npm install mongoose

## 引包

var mongoose = require('mongoose');  //不需要引mongodb了

## schema

```js
//先定义schema，然后才能创建模型model
var studentSchema = new mongoose.Schema({
    name     :  {type : String},
    age      :  {type : Number},
    sex      :  {type : String}
});
```

## model

```js
var mongoose = require('mongoose');
//创建数据库连接
var db = mongoose.createConnection('mongodb://127.0.0.1:27017/数据库名字');

//定义model，就像一个类
var studentModel = db.model('集合名', studentSchema);	//集合名会被改为小写复数存入数据库
```

## 实例化model

```js
//就像对象实例化一个类一样
var m = new studentModel();
m.name = 'Statue of Liberty';
m.age = 125;
m.sex = '男';
m.save(callback);
//也可以这样写
var m = new Student({name:'Statue of Liberty',age:125,sex:'男'});
m.save(callback);
```

## 无实例直接创建

```js
//有点像调用类的静态方法
studentModel.create({ name:'Statue of Liberty',age:125,sex:'男' }, function (err, data) {
  if (err) return handleError(err);
  // saved!
})
```

##整个代码

```js
const mongoose = require('mongoose'); 
const schema = new mongoose.Schema({ });
const model = mongoose.model('集合名',schema);
module.exports = model;
```



## 增删改查

### 增

```js
var TestEntity = new TestModel({
    name : "helloworld",
    age  : 28,
    email: "helloworld@qq.com"
});
TestEntity.save(function(error,doc){
  if(error){
     console.log("error :" + error);
  }else{
     console.log(doc);
  }
})
```

或者

```js
TestModel.create({ name:"model_create", age:26}, function(error,doc){
    if(error) {
        console.log(error);
    } else {
        console.log(doc);
    }
});
```

### 删

model.remove(查询条件,callback);

```js
TestModel.remove({_id : '5add9fb3936e5928ac206142'},function(err){
	if (err) {
		console.log('删除失败')
	}else{
		console.log('删除成功')
	}
})
```

### 改

model.update(查询条件,更新对象,callback);

```js
var conditions = {_id : '5add9fb3936e5928ac206142'};
 
var update = {email:'haha!'};
 
TestModel.update(conditions, update, function(error){
    if(error) {
        console.log(error);
    } else {
        console.log('Update success!');
    }
});
```

### 查

```js
TestModel.find({name:'helloworld'},function(err,doc){
	if(err)
		console.log(err)
	else
		console.log(doc)
})
```


## 外键填充 

populate方法

**参数**

- path 外键的索引
- [select] «Object|String» 表关联查询要选择的字段
- [model] «Model» 表关联的 model 。如果没有指定，将以 Schema 中 `ref` 字段为名称查找 model 进行关联。
- [match] «Object» population 查询的条件
- [options] «Object» population 查询的选项 (sort 等)

**示例**

```js
model.
  findOne({ title: 'Casino Royale' }).
  populate('author').
  exec(function (err, story) {

  });
```



