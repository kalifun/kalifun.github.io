---
title: zTree 文件树
date: 2019-06-03 09:15:00
categories: 前端
tags:
  - ztree
---

# zTree 文件树

## zTree简介

> #### zTree 是一个依靠 jQuery 实现的多功能 “树插件”。优异的性能、灵活的配置、多种功能的组合是 zTree 最大优点。

**这当然是官方给的说法，不过我也是这么觉得。当你使用了会发现还是很有趣的。**

## 运用场景

运用场景其实并不广泛，一般在后台管理页面出现的身影比较多。简单的结构我们都是悬着表格来展现。像文件浏览，资产管理，权限管理就可以看到它的身影了。

## 使用步骤

在官网很多实例我们下载文件时都有，我们可以查看他们是如何实现的。

### 引用

```html
<link rel="stylesheet" href="css/metroStyle/metroStyle.css">   //metro风格
<script src="js/jquery.ztree.all.min.js"></script>
请不要忘记了加载jQuery。
```

```html
<div class="col-lg-1 modal-content" >
	<ul id="regionZTree" class="ztree"></ul>
</div>
```

### 自定义Js

```js
zTreeObj = $.fn.zTree.init($("#regionZTree"), setting, treelist);
var setting = {
    view: {
        addHoverDom: addHoverDom,   //添加创建文件夹按钮
        removeHoverDom: removeHoverDom,    //去除创建文件夹按钮
        dblClickExpand: false,//双击节点时，是否自动展开父节点的标识
        showLine: true,//是否显示节点之间的连线
        fontCss:{'color':'black','font-weight':'bold'},//字体样式函数
        selectedMulti: true //设置是否允许同时选中多个节点
    },
    check:{
        //chkboxType: { "Y": "ps", "N": "ps" },
        chkboxType: { "Y": "", "N": "" },
        chkStyle: "checkbox",//复选框类型
        enable: false //每个节点上是否显示 CheckBox
    },
    edit:{
        enable: true,
        editNameSelectAll: false,
        showRemoveBtn : false,
        showRenameBtn : false,
        removeTitle : "remove",
        renameTitle : "rename"
    },
    data: {
        simpleData: {//简单数据模式
            enable:true,
            idKey: "id",
            pIdKey: "IPARENTID",
            rootPId: null
        }
    },
    callback: {
        // beforeExpand:zTreeBeforeExpand, // 用于捕获父节点展开之前的事件回调函数，并且根据返回值确定是否允许展开操作
        beforeRightClick: zTreeBeforeRightClick,  //用于捕获 zTree 上鼠标右键点击之前的事件回调函数，并且根据返回值确定触发 onRightClick 事件回调函数
        onClick: Spanningtree, //用于捕获节点被点击的事件回调函数
        onRightClick: Downtreefile,   //用于捕获 zTree 上鼠标右键点击之后的事件回调函数
        onDrop: Throwfile,   //用于捕获节点拖拽操作结束的事件回调函数
    }
};
```

## zTree解析

```html
zTreeObj = $.fn.zTree.init($("#regionZTree"), setting, treelist); 
//第一个参数是我需要显示tree的地方，第二个参数时我们针对tree的设置，第三个参数时数据源
```

### 数据类型

**从上面可以看到我们还需要传入一个参数treelist。那treelist具体是什么一个形式呢？**

```js
var nodes = [
	{name: "父节点1", children: [
		{name: "子节点1"},
		{name: "子节点2"}
	]}
];
```

```js
var nodes = [
	{id:1, pId:0, name: "父节点1"},
	{id:11, pId:1, name: "子节点1"},
	{id:12, pId:1, name: "子节点2"}
];
简单模式的 JSON 数据需要使用 id / pId 表示节点的父子包含关系.
```

**个人比较中意第一种，因为这个就像json一样，首先我们也方便构造，也很直观可以看清父子的关系。**

### Setting

> ##### 具体的setting请查看官方的API文档。

- view(视图类)

  视图相关的设置。这个也类似键值对，你只需要知道Key的使用方法，value则是自定的方法名。

- callback(回调机制)

##### 例子：

```js
function zTreeOnClick(event, treeId, treeNode) {
    alert(treeNode.tId + ", " + treeNode.name);
};
var setting = {
	callback: {
		onClick: zTreeOnClick
	}
};
```

- **event**

  **记录触发的事件**

- **treeId**

  **触发的treeid**

- **treeNode**

  **这个将记录我们传入的文件数据。**

