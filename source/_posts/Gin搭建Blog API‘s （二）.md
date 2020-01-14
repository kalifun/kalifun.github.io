---
title: Gin搭建Blog API‘s （二）
date: 2018-10-26 16:05:00
categories: Golang
tags:
  - golang
  - gin
---
# Gin搭建Blog API‘s （二）
## 按照[Gin搭建Blog API's (一)](https://github.com/EDDYCJY/blog/blob/master/golang/gin/2018-02-16-Gin%E5%AE%9E%E8%B7%B5-%E8%BF%9E%E8%BD%BD%E4%B8%89-%E6%90%AD%E5%BB%BABlogAPIs-02.md)教程来的。此为原作者。
## 定义接口
- 获取标签列表：GET("/tags")
- 新建标签：POST("/tags")
- 更新指定标签：PUT("/tags/:id")
- 删除指定标签：DELETE("/tags/:id")

## 编写路由空壳
在routers下创建api目录，我们当前第一个版本，因此在api下新建v1目录，再创建tag.go。
```go
package v1

import (
	"github.com/gin-gonic/gin"
)

func GetTags(c *gin.Context) {
	// 获取tags
}

func AddTags(c *gin.Context) {
	// 新增tags
}

func EditTags(c *gin.Context) {
	// 更新tags
}

func DeleteTags(c *gin.Context) {
	// 删除tags
}
```
## 注册路由
修改router.go
```go
package routers

import (
	"gin-blog/pkg/setting"
	"github.com/gin-gonic/gin"
	"gin-blog/routers/api/v1"
)

func InitRouter() *gin.Engine {
	r := gin.New()
	r.Use(gin.Logger())
	r.Use(gin.Recovery())

	gin.SetMode(setting.RunModel)
	
	apiv1 := r.Group("/api/v1")
	{
		apiv1.GET("/tags",v1.GetTags)
		apiv1.POST("/tags",v1.AddTags)
		apiv1.PUT("/tags/:id",v1.EditTags)
		apiv1.DELETE("/tags/:id",v1.DeleteTags)
	}

	// r.GET("/test",func(c *gin.Context){
	// 	c.JSON(200,gin.H{
	// 		"message":"ok",
	// 	})
	// })
	return r
}
```
当出现下面这样，说明我们设计的路由规则通过了。  
[![answer.png](https://i.postimg.cc/DwvV9gqP/answer.png)](https://postimg.cc/0bBHm7Xz)
#### 目前的目录树是：
```go
│  main.go
│  test.go
│
├─conf
│      app.ini
│
├─middleware
├─models
│      models.go
│
├─pkg
│  ├─e
│  │      code.go
│  │      msg.go
│  │
│  ├─setting
│  │      setting.go
│  │
│  └─util
│          pagination.go
│
├─routers
│  │  router.go
│  │
│  └─api
│      └─v1
│              tag.go
│
└─runtime
```  
## 编写标签列表models逻辑
下载依赖包[validation](https://beego.me/docs/mvc/controller/validation.md)。用于数据验证和错误收集的模块。
```go
go get github.com/astaxie/beego/validation
```
 在models目录下创建tag.go
```go
package models

type Tag struct {
	Model

	Name string `json:"name"`
	CreatedBy string `json:"created_by"`
	ModifiedBy string `json:"modified_by"`
	State int `json:"state"`

}

func GetTags(pageNum int, pageSize int, maps interface {}) (tags []Tag) {
	db.Where(maps).Offset(pageNum).Limit(pageSize).Find(&tags)

	return
}

func GetTotal(maps interface {}) (count int) {
	db.Model(&Tag{}).Where(maps).Count(&count)

	return
}
```
-  创建struct{}，提供给gorm使用。并给予了附属属性json，在c.json的时候会自动转换格式。
-  可能会有的初学者看到return，而后面没有跟着变量，会不理解；其实你可以看到在函数末端，我们已经显示声明了返回值，这个变量在函数体内也可以直接使用，因为他在一开始就被声明了
-  有人会疑惑db是哪里来的；因为在同个models包下，因此db *gorm.DB是可以直接使用的

## 编写标签列表的路由逻辑
编写获得标签列表的接口，修改routers/api/v1/tag.go
```go
package v1

import (
	"net/http"
	"gin-blog/pkg/e"
	"gin-blog/pkg/util"
	"gin-blog/models"
	"gin-blog/pkg/setting"
	"github.com/Unknwon/com"
	"github.com/gin-gonic/gin"
)

func GetTags(c *gin.Context) {
	// 获取tags
	name := c.Query("name")
	maps := make(map[string]interface{})
	data := make(map[string]interface{})

	if "name" != "" {
		maps["name"] = name
	}

	var state int = -1
	if arg := c.Query("state");arg != "" {
		state = com.StrTo(arg).MustInt()
		maps["state"] = state
	}

	code := e.SUCCESS

	data["lists"] = models.GetTags(util.Getpag(c),setting.Pagesize,maps)
	data["total"] = models.GetTotal(maps)

	c.JSON(http.StatusOK,gin.H{
		"code" : code,
		"msg" : e.GetMsg(code),
		"data" : data,
	})
}

func AddTags(c *gin.Context) {
	// 新增tags
}

func EditTags(c *gin.Context) {
	// 更新tags
}

func DeleteTags(c *gin.Context) {
	// 删除tags
}
```
-  c.Query可用于获取?name=test&state=1这类URL参数，而c.DefaultQuery则支持设置一个默认值.
-  code变量使用了e模块的错误编码，这正是先前规划好的错误码，方便排错和识别记录.
-  util.GetPag保证了各接口的page处理是一致的
-  c *gin.Context是Gin很重要的组成部分，可以理解为上下文，它允许我们在中间件之间传递变量、管理流、验证请求的JSON和呈现JSON响应

 当我go run main.go的时候出现了错误。
```go
$ go run main.go
# gin-blog/models
models\models.go:27:14: cannot refer to unexported name setting.cfg
models\models.go:27:14: undefined: setting.cfg
```
经过各种排查，终于知道了，当初我定义config *ini.File.而模块中导出的函数必须为字母大写。
现在我们再运行程序。
```go
go run main.go
curl 127.0.0.1:8000/api/v1/tags
{"code":200,"data":{"lists":[],"total":0},"msg":"ok"}
```
## 编写新增标签的models逻辑
 修改models目录下的tag.go文件。(新增)
```go
func ExistTagByName(name string) bool {
	var tag Tag
	db.Select("id").Where("name = ?",name).First(&tag)
	if tag.ID > 0 {
		return true
	}
	return false
}

func AddTag(name string, state int, createdBy string) bool{
	db.Create(&Tag {
		Name : name,
		State : state,
		CreatedBy : createdBy,
	})

	return true
}
```
## 编写新增标签的路由逻辑。
修改routers目录下v1下的tag.go。
```go
func AddTags(c *gin.Context) {
	// 新增tags
	name := c.Query("name")
	state := com.StrTo(c.DefaultQuery("state", "0")).MustInt()
	createdBy := c.Query("created_by")

	valid := validation.Validation{}
	valid.Required(name, "name").Message("名称不能为空")
	valid.MaxSize(name, 100, "name").Message("名称最长为100")
	valid.Required(createdBy, "created_by").Message("创建人不能为空")
	valid.MaxSize(createdBy, 100, "created_by").Message("创建人不能大于100")
	valid.Range(state, 0, 1, "state").Message("状态只能0或1")

	code := e.INVALID_PARAMS
	if !valid.HasErrors() {
		if !models.ExistTagByName(name) {
			code = e.SUCCESS
			models.AddTag(name, state, createdBy)
		} else {
			code = e.ERROR_EXIST_TAG
		}
	} else {
		for _, err := range valid.Errors {
			log.Fatalf("fail %v", err.Message)
			// logging.Info(err.Key,err.Message)
		}
	}
	c.JSON(http.StatusOK, gin.H{
		"code": code,
		"msg":  e.GetMsg(code),
		"data": make(map[string]string),
	})

}
```
#### 编写models callbacks
但是这个时候大家会发现，我明明新增了标签，但created_on居然没有值，那做修改标签的时候modified_on会不会也存在这个问题？为了解决这个问题，我们需要打开models目录下的tag.go文件，修改文件内容（修改包引用和增加2个方法）：
```go
func (tag *Tag) BeforeCreate(scope *gorm.Scope) error {
    scope.SetColumn("CreatedOn", time.Now().Unix())

    return nil
}

func (tag *Tag) BeforeUpdate(scope *gorm.Scope) error {
	scope.SetColumn("ModifiedOn", time.Now().Unix())

	return nil
}
```
