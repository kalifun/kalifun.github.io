---
title: Bootstrap Table 
date: 2019-06-03 09:15:00
categories: 前端
tags:
  - boostatrp	
---

# Bootstrap Table 

## 一，什么是Bootstrap Table

**其实从英文翻译就得知了，是一款Bootstrap的表格插件。**

**Bootstrap Table可用快速的建表，查询，分页，排序，等一系列功能。**

## 二，Bootstrap Table模式

> Boostatrp Table分为两种模式：客户端(client)模式,服务端(server)模式。

- **客户端**

  **通过数据接口将服务器需要加载的数据一次性展现出来，然后装换成Json然后生成table。我们可以自己定义显示行数，分页等。此时就不再会向服务器发送请求了。**

- **服务器**

  **根据设定的每页记录数和当前显示页，发送数据到服务器进行查询。**

## 三，Bootstrap Table使用

#### 下载插件[Bootstrap Table](https://bootstrap-table.com/)

#### 引用插件

```html
<link rel="stylesheet" href="css/bootstrap-table.min.css">
<script src="js/bootstrap-table.min.js"></script>
<script src="js/bootstrap-table-zh-CN.min.js"></script>    //中文表格不乱码
```

**自定义JS**

```javascript
var $newtable;
function InitMainTable(data) {    
    // console.log(data);

    window.operateEvents = {
        'click .download': function (e,value,row,index) {
            console.log(row);
			xxx();
        }
    }


    $newtable = $('#tb_departments').bootstrapTable({
        // url: '/Home/GetDepartment',         //请求后台的URL（*）
        // method: 'get',                      //请求方式（*）
        data: data,
        silent: true,
        toolbar: '#toolbar',                //工具按钮用哪个容器
        striped: true,                      //是否显示行间隔色
        cache: false,                       //是否使用缓存，默认为true，所以一般情况下需要设置一下这个属性（*）
        pagination: true,                   //是否显示分页（*）
        sortable: false,                     //是否启用排序
        sortOrder: "asc",                   //排序方式
        queryParams: function (params) {
            var temp = {   //这里的键的名字和控制器的变量名必须一直，这边改动，控制器也需要改成一样的
                limit: params.limit,   //页面大小
                offset: params.offset,  //页码s
                search: $('#productLine').val()
            };
            console.log(temp);
            return temp;
        },//传递参数（*）
        sidePagination: "client",           //分页方式：client客户端分页，server服务端分页（*）
        pageNumber:1,                       //初始化加载第一页，默认第一页
        pageSize: 6,                       //每页的记录行数（*）
        pageList: [10, 25, 50, 100],        //可供选择的每页的行数（*）
        search: true,                       //是否显示表格搜索，此搜索是客户端搜索，不会进服务端，所以，个人感觉意义不大
        strictSearch: true,
        showColumns: true,                  //是否显示所有的列
        showRefresh: true,                  //是否显示刷新按钮
        minimumCountColumns: 2,             //最少允许的列数
        clickToSelect: true,                //是否启用点击选中行
        height: 500,                        //行高，如果没有设置height属性，表格自动根据记录条数觉得表格高度
        uniqueId: "ID",                     //每一行的唯一标识，一般为主键列
        showToggle:true,                    //是否显示详细视图和列表视图的切换按钮
        cardView: false,                    //是否显示详细视图
        detailView: false,                   //是否显示父子表
        showExport: true,                     //是否显示导出
        exportDataType: "basic",              //basic', 'all', 'selected'.
        columns: [{
            checkbox: true
        }, {
            field: 'Key', title: '文件路径'
        }, {
            field: 'Size', title: '文件大小'
        },{
            field: 'X_COS_META_TAG1', title: '标签#1' , formatter: function (value, row, index) {
                
            }
        },{
            field: 'notice', title: '备注'
        },{
            field: 'Button',title:"操作",align: 'center',events:operateEvents,formatter:function(value,row,index){
                var download = '<button type="button" class="btn btn-danger download">下载</button>'
                return download;
            }
        } 
    ]
    });
}

```

**上面的注释其实已经很清楚明了了。我把它写成了一个函数的形式。这个根据自己的情况而定。我是为了需要复用的时候只需要调用函数就可以了。**

## Bootstrap Table拆解

```javascript
$('#tb_departments').bootstrapTable({})
```

**这个就像table的入口一样。也是初始化表格一样。此时对应的html是**

```html
<table id="tb_departments" data-filter-control="true" data-show-columns="true"></table>
```

------

```js
columns:[{field: 'Key', title: '文件路径',formatter: function(value,row,index){} }]
```

- **columns** 
  - **field json中键值对中的Key**
  - **title是表格头显示的内容**
  - **formatter 是一个函数类型。当我们对数据内容需要修改时(例：编码转换).**

------

```js
events:operateEvents
 window.operateEvents = {
        'click .download': function (e,value,row,index) {
            console.log(row);
			xxx();
        }
   }
```

**事件触发器，因为很多时候我们需要针对表格进行处理，所以事件触发器是一个不错的选择。它可以记录我们的行数据，可以利用触发器进行函数执行等。**

**其余内容查看注释选择使用就可以了。**

## 四，表格导出

```html
<script src="js/bootstrap-table-export.js"></script> 
```

```js
showExport: true,                     //是否显示导出
```

## 5，总结

**其实只是简单的阐述如何使用，你可以去查看下载的文件，你会发现Bootstrap 还有超多的插件我们并没有使用。后续我使用到了新的我再进行补充。**