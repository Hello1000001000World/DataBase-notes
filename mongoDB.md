## 安装

### 下载地址

<https://www.mongodb.com/download-center#community>

### 安装教程

http://www.runoob.com/mongodb/mongodb-window-install.html

### 第三方mongoose文档

http://mongoosejs.com/docs/index.html



## 运行

如果配置完环境变量,可以直接在命令行输入mongod



## 使用

### 连接数据库



### 创建集合



###建立连接

```js
//引用mongoose
var mongoose = require("mongoose");
//创建连接
// var db = mongoose.connect("mongodb://user:pass@localhost:port/database");
mongoose.connect("mongodb://127.0.0.1:27017/数据库名");
//建立连接
mongoose.connection.on("open", function () {
    console.log("------数据库连接成功！------");
});
```

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

## Schema

var TestSchema = new mongoose.Schema({
    name : { type:String },//属性name,类型为String
    age  : { type:Number, default:0 },//属性age,类型为Number,默认为0
    time : { type:Date, default:Date.now },
    email: { type:String,default:''}
});

//实例化Schema,参数一为 数据库集合名
var TestModel = mongoose.model("test1", TestSchema);

//修改定义过的Schema
//TempSchema.add({ name: 'String', email: 'String', age: 'Number' });

//Schema提供公共方法
// TestSchema.method('say', function () {
//   console.log('Trouble Is A Friend');
// })
// var say = mongoose.model('say', TestSchema);
// var lenka = new say();
// lenka.say(); //Trouble Is A Friend


//Schema静态方法
// TestSchema.static('findByName', function (name, callback) {
//     return this.find({ name: name }, callback);
// });
// var TestModel = db.model("test1", TestSchema );
// TestModel.findByName('tom', function (err, docs) {
//  //docs所有名字叫tom的文档结果集
// });

//为Schema模型追加speak方法
// TestSchema.methods.speak = function(){
//   console.log('我的名字叫'+this.name);
// }

// var TestModel = db.model('test1',TestSchema);
// var TestEntity = new TestModel({name:'Lenka'});
// TestEntity.speak();//我的名字叫Lenka



## 游标

//限制数量：find(Conditions,fields,options,callback);
Model.find({},null,{limit:20},function(err,docs){
    //输出20个结果
});

//跳过数量：find(Conditions,fields,options,callback);
//如果查询结果数量中少于4个的话，则不会返回任何结果。
Model.find({},null,{skip:4},function(err,docs){
    console.log(docs);
});

//排序find(Conditions,fields,options,callback);
//1是升序，-1是降序。
Model.find({},null,{sort:{age:-1}},function(err,docs){
  //查询所有数据，并按照age降序顺序返回数据docs
});

## 高级

//属性过滤 find(Conditions,field,callback);
//第一个参数为查询条件,第二个参数为过滤条件(1:返回,0:不返回),若不填则返回全部
Model.find({},{name:1, age:1, _id:0}，function(err,docs){
   //docs 查询结果集
})

//查询结果集第一条数据
TestModel.findOne({ age: 27}, function (err, doc){
   // 查询符合age等于27的第一条数据
   // doc是查询结果
});

//查询单条数据 findById(_id, callback);
TestModel.findById('obj._id', function (err, doc){
 //doc 查询结果文档
});   

/*
"$lt"(小于)，
"$lte"(小于等于),
"$gt"(大于)，
"$gte"(大于等于)，
"$ne"(不等于)，
"$in"(可单值和多个值的匹配)==，
"$or"(查询多个键值的任意给定值)||，
"$exists"(表示是否存在的意思)"$all"。
*/

//查询大于小于不等于
Model.find({"age":{"$gt":18}},function(error,docs){
   //查询所有nage大于18的数据
});
Model.find({"age":{"$lt":60}},function(error,docs){
   //查询所有nage小于60的数据
});
Model.find({"age":{"$gt":18,"$lt":60}},function(error,docs){
   //查询所有nage大于18小于60的数据
});
Model.find({ age:{ $ne:24}},function(error,docs){
    //查询age不等于24的所有数据
});
Model.find({name:{$ne:"tom"},age:{$gte:18}},function(error,docs){
  //查询name不等于tom、age>=18的所有数据
});

//查询等于
Model.find({ age:{ $in: 20}},function(error,docs){
   //查询age等于20的所有数据
});
Model.find({ age:{$in:[20,30]}},function(error,docs){
  //可以把多个值组织成一个数组
}); 

Model.find({"$or":[{"name":"yaya"},{"age":28}]},function(error,docs){
  //查询name为yaya或age为28的全部文档
});

//查询存在或不存在该属性的文档
Model.find({name: {$exists: true}},function(error,docs){
  //查询所有存在name属性的文档
});
Model.find({telephone: {$exists: false}},function(error,docs){
  //查询所有不存在telephone属性的文档
});