---
title: Vue初识
date: 2019-06-17 09:15:00
categories: 前端
tags:
  - vue
---

# Vue初识

## 基本指令

### v-cloak

> #### 在Vue实例结束编译时从绑定的HTML元素上移除，经常和CSS的display:none 配合使用。

```vue
<div id = 'app' v-cloak>
    {{message}}
</div>
<script>
	var app = new Vue({
        el: '#app',
        data: {
            message: "this is message"
        }
    })
</script>
```

**当我们加载程序的时候，如果遇见网上较慢情况，我们将会看到{{message}}，所以我们需要结合CSS来让它不显示。**

```html
[v-cloak] {
	dispaly: none;
}
```

### v-once

> #### 定义它的元素或组件只能渲染一次，包括元素或所有组件的字节点。渲染后将不再渲染，视为静态文件。

```vue
<div id="app">
        <span v-once>{{message}}</span>
        <div v-once>
            <span> {{ message }} </span>
        </div>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                message: "this is message"
            }
        })
    </script>
```

### v-if,v-else-if,v-else

```vue
    <div id="app">
        <template v-if="type === 'name'">
            <label for="">用户名：</label>
            <input type="text" key="name-input">
        </template>
        <template v-else>
            <label for="">邮箱名：</label>
            <input type="text" key="mail-input">
        </template>
        <button @click="buttonclick">切换类型</button>
    </div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            type: 'name'
        },
        methods: {
            buttonclick: function () {
                this.type = this.type === 'name' ? 'mail' : 'name' ;
            }
        }
    })
</script>
```

### v-show

> #### v-show就是改变CSS的display

```vue
    <div id="app">
        <h1 v-show="show">
            this is titile
        </h1>
    </div>
<script>
    var app = new  Vue({
        el: '#app',
        data: {
            show: false
        }
    })
</script>
```

### v-for

> #### 顾名思义就是当我们获得到一个List或者数组进行for循环获得具体内容

```vue
<body>
    <div id="app">
        <ul>
            <li v-for="(book,index) in books">{{index}} - {{book.name}}</li>
        </ul>
        <ul>
            <template v-for="book in books">
                <li>书名：{{book.name}}</li>
                <li>作者:{{book.author}}</li>
            </template>
        </ul>
        <span v-for="u in user">
            {{u}}
        </span>
    </div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            books: [
                {
                    name: '《Vue 实战》',
                    author: 'kalifun'
                },
                {
                    name: '《Linux 就该这么学》',
                    author: 'kali'
                }
            ],
            user: {
                name: 'kali',
                age: 23,
                language: 'python'
            }

        }
    })
</script>
</body>
```

```html
<li>0 - 《Vue 实战》</li>
<li>1 - 《Linux 就该这么学》</li>

<li>书名：《Vue 实战》</li>
<li>作者:kalifun</li>
<li>书名：《Linux 就该这么学》</li>
<li>作者:kali</li>

kali 23 python
```

#### 数组更新

##### 变异方法

**Vue的核心是数据和视图双向绑定的，当我们更新了数据，视图也会自动更新。**

- **push**

  ```html
  app.books.push({name:'《CSS》',author:'kali'});
  ```

- **pop()**

  ```
  app.books.pop();
  //删除最后一组元素
  ```

- **shift()**

  ```
  app.books.shift();
  //把数组的第一个元素从其中删除
  ```

- **unshift()**

  ```
  app.books.unshift({name:'《CSS》',author:'kali'});
  //在数组的开始添加新的元素
  ```

- **splice()**

  ```
  从数组中添加/删除项目，然后返回被删除的项目。
  ```

- **sort()** 

  ```
  app.books.sort();
  //返回数组排序后的结果
  ```

- **reverse()**

  ```
  app.books.reverse();
  //用于点到数组中元素的顺序
  ```

##### 替换数组

- **filter()**

  ```
  创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
  ```

- **concat()**

  ```
  用于连接两个或多个数组。
  ```

- **slice()**

  ```
   方法可从已有的数组中返回选定的元素。
  ```

### v-on

> #### 对事件进行绑定

```vue
<div id="on">
	<p>{{ message }}</p>
	<button v-on:click="handle">提交</button>
</div>
<script>
    var app = new Vue({
        el:"#on",
        data:{
            message:'Hello Vue.js!'
        },
        methods: {
            handle: function() {
                this.message = this.message.split('').reverse().join('')
            }
        },
    })
</script>
```

#### 修饰符

- **.stop**
- **.prevent**
- **.capture**
- **.self**
- **.once**

```vue
//阻止单击事件冒泡
<a @click.stop='xxxx'></a>
//提交事件不再重载页面
<form @submit.prevent='xxx'></form>
// 修饰符串联
<form @submit.stop.prevent='xxx'></form>
//添加事件侦听器时使用事件捕获器
<div @click.capture='xxx'></div>
//只当事件在该元素本身（而不是子元素)触发时触发回调
<div @click.self='xxx'></div>
// 只触发一次
<div @click.onse='xxx'></div>
```

#### KeyCode

- **.enter**
- **.tab**
- **.delete**
- **.esc**
- **.space**
- **.up**
- **.down**
- **.left**
- **.right**



------



## 练习

**将一个商品List制作出一个表格，可以调整数量，并计算出总价格。**

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <table border="1">
            <thead>
                <tr>
                    <th></th>
                    <th>商品名称</th>
                    <th>商品价格</th>
                    <th>商品数量</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(tb,index) in list">
                    <td>{{index+1}}</td>
                    <td>{{tb.name}}</td>
                    <td>{{tb.price}}</td>
                    <td>
                        <button @click="reduce(index)">-</button>
                        {{tb.count}}
                        <button @click="addcount(index)">+</button>
                    </td>
                    <td>
                        <button @click="removevalue(index)">移除</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <div>总价格：{{allprice}}</div>
    </div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            list: [
                {
                    name: 'ipad',
                    price: 5000,
                    count: 4
                },
                {
                    name: 'iphone',
                    price: 6000,
                    count: 1
                },
                {
                    name: 'HuaWei',
                    price: 5000,
                    count: 5
                },
                {
                    name: 'xiaomi',
                    price: 4000,
                    count: 3
                }
            ]
        },
        methods: {
            reduce: function (index) {
                if (this.list[index].count === 1) return;
                this.list[index].count --;
            },
            addcount: function (index) {
                this.list[index].count ++ ;
            },
            removevalue: function (index) {
                this.list.splice(index,1) ;
            }
        },
        computed: {
            allprice: function () {
                var total = 0;
                for (var i = 0;i < this.list.length;i++){
                    var iteam = this.list[i];
                    total += iteam.price * iteam.count;
                }
                return total.toString();
            }
        }
    })
</script>
</body>
</html>
```



------



## 表单与v-model

### 基本用法

```vue
<div id='app'>
    <input type='text' v-model='message'>
    {{message}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: ''
        }
    })
</script>
```

**v-model也支持语法糖，可以将v-model改成@input**

```vue
<div id='app'>
    <input type='text' @input='inputinfo'>
    {{message}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: ''
        },
        methods: {
            inputinfo: function(e){
                this.message = e.target.value;
            }
        }
    })
</script>
```

#### 单选按钮

**当单选按钮时，不需要使用v-model，使用v-bind绑定一个布尔类型的值。**

```vue
<div id='app'>
    <input type='radio' :checked='picked'>
    <label>单选按钮</label>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            picked: true
        }
    })
</script>
```

**如果是组合使用来实现互斥的效果，就需要v-model配合value来实现了**

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <input type="radio" v-model="checked" value="html">
        <label for="html">HTML</label><br>
        <input type="radio" v-model="checked" value="js">
        <label for="js">Js</label><br>
        <input type="radio" v-model="checked" value="css">
        <label for="css">css</label><br>
        <p>选择的是：{{checked}}</p>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                checked: 'js'
            }
        })
    </script>
</body>
</html>
```

#### 复选框

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
<div id="app">
    <input type="checkbox" v-model="checked" value="html">
    <label for="html">Html</label><br>
    <input type="checkbox" v-model="checked" value="js">
    <label for="js">Js</label><br>
    <input type="checkbox" v-model="checked" value="css">
    <label for="css">Css</label><br>
    <p>选择的是：{{checked}}</p>
</div>
<script>
    var app = new Vue({
       el: '#app',
       data: {
           checked: ['html','js']
       }
    });
</script>
</body>
</html>
```

#### 选择列表

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <select name="" id="" v-model="checked" multiple>
            <option value="html">HTML</option>
            <option value="js">Js</option>
            <option value="css">Css</option>
        </select>
        <p>选择的是：{{checked}}</p>
    </div>
    <script>
        var app = new Vue({
           el: '#app',
           data: {
               checked: ['js','css']
           }
        });
    </script>
</body>
</html>
```

**结合v-bind来动态输出**

```vue
<!DOCTYPE html> 
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <select name="" id="" v-model="checked">
            <option v-for="option in options" :value="option.value">{{option.text}}</option>
        </select>
        <p>选择的是：{{checked}}</p>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                checked: 'html',
                options: [
                    {
                        text: 'Html',
                        value: 'html'
                    },
                    {
                        text: 'Js',
                        value: 'js'
                    },
                    {
                        text: 'Css',
                        value: 'css'
                    }
                ]
            }
        });
    </script>
</body>
</html>
```

### 绑定值

#### 单选按钮

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <input type="radio" v-model="checked" :value="value">
        <label for="">单选框</label>
        <p>{{checked}}</p>
        <p>{{value}}</p>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                checked: false,
                value: 123
            }
        })
    </script>
</body>
</html>
```

#### 复选框

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <input type="checkbox" v-model="checked" :true-value="value1" :false-value="value2">
        <label for="">复选框</label>
        <p>{{checked}}</p>
        <p>{{value1}}</p>
        <p>{{value2}}</p>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
               checked: false,
                value1: 'a',
                value2: 'b'
            }
        });
    </script>
</body>
</html>
```

#### 选择列表

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
</head>
<body>
    <div id="app">
        <select name="" id="" v-model="selected">
            <option :value="{ number: 123}">123</option>
        </select>
        {{selected.number}}
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                selected: ''
            }
        });
    </script>
</body>
</html>
```

### 修饰符

- **.lazy**

  **使用了lazy将不再是实时同步，需要失焦或者按enter才会实行同步**

  ```vue
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <script src="https://cdn.bootcss.com/vue/2.6.10/vue.js"></script>
  </head>
  <body>
      <div id="app">
          <input type="text" v-model.lazy="message">
          {{message}}
      </div>
      <script>
          var app = new Vue({
              el: '#app',
              data: {
                  message: ''
              }
          })
      </script>
  </body>
  </html>
  ```

- **.number**

  **可以将输入的装换成Number类型。如果不使用时虽然我们输入的是数字，但它会被转成string类型。**

  ```vue
  <div id="app">
      <input type="text" v-model.number="message">
      {{typeof message}}
  </div>
  <script>
      var app = new Vue({
          el: '#app',
          data: {
              message: '123'
          }
      })
  </script>
  ```

- **.trim**

  **可以自动过滤掉首尾输入的空格。**

  ```vue
  <div id="app">
      <input type="text" v-model.trim="message">
      {{ message}}
  </div>
  <script>
      var app = new Vue({
          el: '#app',
          data: {
              message: ''
          }
      })
  </script>
  ```



------



## 组件详解

### 组件与复用

#### 组件使用

> ##### 组件需要注册后才可以使用。注册有两种：全局注册和局部注册。下面是全局注册的例子：

```vue
Vue.component('组件名称',{})
```

**在组件选项中添加template就可以显示组件内容啦。**

```vue
<div id="app">
    <my-component></my-component>
</div>
<script>
    Vue.component('my-component',{
        template: '<div>这是组件</div>'
    })
	var app = new Vue({
        el: '#app'
    })
</script>
```

**渲染得到的是:**

```vue
<div id="#app">
    <div>
        这是组件
    </div>
</div>
```

> ##### 使用components可以实现局部注册。注册后的组件只能在组件的作用域下生效。

```vue
<div id="app">
    <my-component></my-component>
</div>
<script>
 	var child = "<div>这是组件</div>"
	var app = new Vue({
        el: '#app',
        components: {
            'my-component': child
        }
    })
</script>
```

**渲染得到的是:**

```vue
<div id="#app">
    <div>
        这是组件
    </div>
</div>
```

##### 总结

**除了template外，我们还可以使用data,computed等。但是使用data时，和实例不同。data必须是函数，然后讲值return出去。**

```vue
<div id="app">
    <my-component></my-component>
</div>
<script>
    Vue.component('my-component',{
        template: '<div>{{message}}</div>',
        data: function （）{
        	return {
        		message： '这是组件'
    		}
    	}
    })
	var app = new Vue({
        el: '#app'
    })
</script>
```

### 使用props传递参数

#### 基本使用

> ##### 通常父组件的模板中包含子组件，父组件要正向地向子组件传递数据或参数，子组件接收到后根据参数的不同来渲染不同的内容或执行操作。这个正向传递数据的过程就是通过 props 来实现的。

**使用选项 prop 来声明需要从父级接收的数据， prop 的值可以是两种，一种是字符串数组，一种是对象。**

```vue
<div id="app">
    <my-component message="这是组件"></my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['message'],
        template: '<div>{{message}}</div>'
    })
	var app = new Vue({
        el: '#app'
    })
</script>
```

**渲染得到的是:**

```vue
<div id="#app">
    <div>
        这是组件
    </div>
</div>
```

**当要使用Dom模板时，驼峰命名的props名称需要转为短横分隔命名。**

```vue
<div id="app">
    <my-component Component-Text="这是组件"></my-component>
</div>
<script>
    Vue.component('my-component',{
        props: ['ComponentText'],
        template: '<div>{{ComponentText}}</div>'
    })
	var app = new Vue({
        el: '#app'
    })
</script>
```

**渲染得到的是：**

```vue
<div id="#app">
    <div>
        这是组件
    </div>
</div>
```

**有时候传递的参数不是固定的，这时候我们需要使用v-bind来动态绑定props的值。**

```vue
    <div id="app">
        <input type="text" v-model="inputtext">
        <my-component v-bind:message="inputtext"></my-component>
    </div>
    <script>
        Vue.component('my-component',{
            props: ['message'],
            template: '<div>{{message}}</div>'
        })
        var app = new Vue({
           el: '#app',
            data: {
               inputtext: '这是组件'
            }
        });
    </script>
```

#### 单项数据流

> ##### Vue2.x 通过 props 传递数据是单向的了， 也就父组件数据变化时会传递给子组件，但是反过来不行。

```vue
<div id="app">
        <mycomponent :initcount="1"></mycomponent>
    </div>
    <script>
        Vue.component('mycomponent',{
           props: ['initcount'],
            template: '<div>{{count}}</div>',
            data: function () {
                return {
                    count: this.initcount
                }
            }
        });
        var app = new Vue({
           el: '#app',
        });
    </script>
```

```vue
<div id="app">
        <mycomponent :width="100"></mycomponent>
    </div>
    <script>
        Vue.component('mycomponent',{
           props: ['width'],
            template: '<div :style="style">这是组件</div>',
           computed: {
                style: function () {
                    return {
                        width: this.width + 'px'
                    }
                }
           }
        });
        var app = new Vue({
           el: '#app'
        });
```

#### 数据验证

**验证类型：**

	- **String**
	- **Number**
	- **Boolean**
	- **Object**
	- **Array**
	- **Function**

```vue
    Vue.component('mycomponent',{
        props: {
            // 必须是数字类型
            propA: Number,
            // 必须字符串或者数字类型
            propB: [String,Number],
            // 布尔类型。如果没定义，默认为true
            propC: {
                type: Boolean,
                default: true
            },
            // 数字类型，必须上传
            propD: {
                type: Number,
                required: true
            },
            // 如果是一个数组或者对象，必须要是一个函数来返回
            propE: {
                type: Array,
                default: function () {
                    return []
                }
            }
        }
    });
```

### 组件通信

#### 自定义事件

> ##### 子组件用 $emit（）来触发事件，父组件用$on（）来监昕子组件的事件。

```vue
<div id="app">
        <p>总数：{{total}}</p>
        <mycomponent @increase="GetTotal" @reduce="GetTotal"></mycomponent>
    </div>
    <script>
        Vue.component('mycomponent',{
            template: '<div><button @click="InCrease">+1</button> 
            			<button @click="Reduce">-1</button></div>',
            data: function () {
                return {
                    counter: 0
                }
            },
            methods: {
                InCrease: function () {
                    this.counter ++;
                    this.$emit('increase',this.counter);
                },
                Reduce: function () {
                    this.counter --;
                    this.$emit('reduce',this.counter);
                }
            }
        });
        var app = new Vue({
           el: '#app',
           data: {
               total: 0
           },
            methods: {
                GetTotal: function (total) {
                    this.total = total;
                }
            }
        });
    </script>
```

#### 使用v-model

```vue
    <div id="app">
        <p>总数：{{total}}</p>
        <mycomponent v-model="total"></mycomponent>
    </div>
    <script>
        Vue.component('mycomponent',{
           template: '<button @click="hander">+1</button>',
            data: function () {
                return {
                    counter: 0
                }
            },
            methods: {
               hander: function () {
                   this.counter++;
                   this.$emit('input',this.counter);
               }
            }
        });
        var app = new Vue({
           el: '#app',
            data: {
                total: 0
            }
        });
    </script>
```

#### 非父子组件通信

> ##### 非父子组件通信通常有两种。①兄弟组件。②跨多级组件。

##### $dispatch() 此功能已经废弃

> ###### $dispatch()用于向上级派发事件，只要是他的父级都可以在vue实例的event接收。

```vue
    <div id="app">
        {{message}}
        <mycomponent></mycomponent>
    </div>
    <script>
        Vue.computed('mycomponent',{
           template: '<button @click="sedmess">传送消息</button>' ,
            methods: {
               sedmess: function () {
                   this.$dispatch('sed-message',"来自内部消息");
               }
            }
        });
        var app = new Vue({
            el: '#app',
            data: {
                message: ''
            },
            event: {
                'sed-message': function (msg) {
                    this.message = msg;
                }
            }
        });
    </script>
```

### 使用slot发布内容

#### 什么是slot

> ##### 当我们多个组件需要组合使用，混合父组件的内容与子组件的模板时，就会用到slot，这个过程叫做内容分发。

- **<app>组件不知道他的挂载点会有什么东西，挂载点的内容由<app>父组件决定。**

- **<app>组件很有可能有他自己的模板。**

  

