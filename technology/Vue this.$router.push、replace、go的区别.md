### Vue this.$router.push、replace、go的区别

#### 1、this.$router.push

**描述：跳转到不同的url，但这个方法会向history添加一个记录，点击后会返回到上一个页面**

##### 用法

```javascript
//字符串
this.$router.push('home')
//对象
this.$router.push({path:'home'})
//命名的路由
this.$router.push({name:'user',params:{userId:123}})
//带查询参数，变成/register?plan=private
this.$router.push({path:'register',query:{plan:'private'}})
```

#### 2、this.$router.replace

**描述：同样是跳转到指定的url，但是这个方法不会向history里面添加新的记录，点击返回，会跳转到上上一个页面。上一个记录是不存在的**

#### 3、this.$router.go

**描述：相当于当前页面向前或向后跳转多少个页面，蕾丝window.hisstory.go(n)。n可为正数或者也可以为负数**

##### 用法

```javascript
//在浏览器记录中前进一步，等同于history.forward()
this.$router.go(1)
//后退一步记录
this.$router.go(-1)
//前进三步记录
this.$router.go(3)
```

