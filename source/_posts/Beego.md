---
title : Beego
date: 2018-11-22 17:28:12
categories: Golang
tags :
  - go
  - golang
---
# Beego
## bee工具简介
### bee工具是一个为了协助开发[beego](https://beego.me/)项目而创建的项目。
## bee安装
```go
go get github.com/beego/bee
```
#### 安装完之后，bee可执行文件默认放到$GOPATH/bin里面，Windows默认已经添加了环境变量。mac安装完成需要自己添加环境变量。
```bash
echo 'export GOPATH=""' >> ~/.zshrc
echo 'export PATH="$GOPATH/bin:$PATH"' >> ~/.zshrc
```
## bee工具命令详解
### bee命令
```bash
$ bee
Bee is a Fast and Flexible tool for managing your Beego Web Application.

USAGE
    bee command [arguments]

AVAILABLE COMMANDS

    version     Prints the current Bee version
    migrate     Runs database migrations
    api         Creates a Beego API application
    bale        Transforms non-Go files to Go source files
    fix         Fixes your application by making it compatible with newer versions of Beego
    dlv         Start a debugging session using Delve
    dockerize   Generates a Dockerfile for your Beego application
    generate    Source code generator
    hprose      Creates an RPC application based on Hprose and Beego frameworks
    new         Creates a Beego application
    pack        Compresses a Beego application into a single file
    rs          Run customized scripts
    run         Run the application by starting a local development server
    server      serving static content over HTTP on port

Use bee help [command] for more information about a command.

ADDITIONAL HELP TOPICS


Use bee help [topic] for more information about that topic.
```
### new命令
#### new命令是用来新建一个web项目，我们执行```bee new <项目名称>```。将会在$GOPATH/src目录下创建文件夹。
```go
$ bee new beeg
______
| ___ \
| |_/ /  ___   ___
| ___ \ / _ \ / _ \
| |_/ /|  __/|  __/
\____/  \___| \___| v1.10.0
2018/10/31 23:20:44 WARN     ▶ 0001 You current workdir is not inside $GOPATH/src.
2018/10/31 23:20:44 INFO     ▶ 0002 Creating application...
	create	 /Users/fun/go/src/beeg/
	create	 /Users/fun/go/src/beeg/conf/
	create	 /Users/fun/go/src/beeg/controllers/
	create	 /Users/fun/go/src/beeg/models/
	create	 /Users/fun/go/src/beeg/routers/
	create	 /Users/fun/go/src/beeg/tests/
	create	 /Users/fun/go/src/beeg/static/
	create	 /Users/fun/go/src/beeg/static/js/
	create	 /Users/fun/go/src/beeg/static/css/
	create	 /Users/fun/go/src/beeg/static/img/
	create	 /Users/fun/go/src/beeg/views/
	create	 /Users/fun/go/src/beeg/conf/app.conf
	create	 /Users/fun/go/src/beeg/controllers/default.go
	create	 /Users/fun/go/src/beeg/views/index.tpl
	create	 /Users/fun/go/src/beeg/routers/router.go
	create	 /Users/fun/go/src/beeg/tests/default_test.go
	create	 /Users/fun/go/src/beeg/main.go
2018/10/31 23:20:44 SUCCESS  ▶ 0003 New application successfully created!
```
```
├── conf
│   └── app.conf
├── controllers
│   └── default.go
├── main.go
├── models
├── routers
│   └── router.go
├── static
│   ├── css
│   ├── img
│   └── js
│   └── reload.min.js
├── tests
│   └── default_test.go
└── views
    └── index.tpl
```
### api命令
#### api命令是用来创建api应用的。
```go
$ bee api beeg
______
| ___ \
| |_/ /  ___   ___
| ___ \ / _ \ / _ \
| |_/ /|  __/|  __/
\____/  \___| \___| v1.10.0
2018/10/31 23:46:46 INFO     ▶ 0001 Creating API...
	create	 /Users/fun/go/src/beeg
	create	 /Users/fun/go/src/beeg/conf
	create	 /Users/fun/go/src/beeg/controllers
	create	 /Users/fun/go/src/beeg/tests
	create	 /Users/fun/go/src/beeg/conf/app.conf
	create	 /Users/fun/go/src/beeg/models
	create	 /Users/fun/go/src/beeg/routers/
	create	 /Users/fun/go/src/beeg/controllers/object.go
	create	 /Users/fun/go/src/beeg/controllers/user.go
	create	 /Users/fun/go/src/beeg/tests/default_test.go
	create	 /Users/fun/go/src/beeg/routers/router.go
	create	 /Users/fun/go/src/beeg/models/object.go
	create	 /Users/fun/go/src/beeg/models/user.go
	create	 /Users/fun/go/src/beeg/main.go
2018/10/31 23:46:46 SUCCESS  ▶ 0002 New API successfully created!
```
```
├── conf
│   └── app.conf
├── controllers
│   ├── default.go
│   ├── object.go
│   └── user.go
├── main.go
├── models
│   ├── object.go
│   └── user.go
├── routers
│   └── router.go
├── static
│   ├── css
│   ├── img
│   └── js
│       └── reload.min.js
├── tests
│   └── default_test.go
└── views
    └── index.tpl
```
### run命令
####  ```bee run```命令是监控beego的项目，但是需要注意的是一定需要在自己创建的目录下才可以运行哦。  
```go
$ bee run
______
| ___ \
| |_/ /  ___   ___
| ___ \ / _ \ / _ \
| |_/ /|  __/|  __/
\____/  \___| \___| v1.10.0
2018/10/31 23:49:08 INFO     ▶ 0001 Using 'beeg' as 'appname'
2018/10/31 23:49:08 INFO     ▶ 0002 Initializing watcher...
beeg/models
beeg/controllers
beeg/routers
beeg
2018/10/31 23:49:11 SUCCESS  ▶ 0003 Built Successfully!
2018/10/31 23:49:11 INFO     ▶ 0004 Restarting 'beeg'...
2018/10/31 23:49:11 SUCCESS  ▶ 0005 './beeg' is running...
2018/10/31 23:49:11.600 [I] [parser.go:112]  generate router from comments
2018/10/31 23:49:11.601 [I] [router.go:269]  /Users/fun/go/src/beeg/controllers no changed
2018/10/31 23:49:11.609 [I] [asm_amd64.s:1333]  http server Running on http://:8080
```
#### 打开浏览器访问```htttp://127.0.0.1:8080```就可以看到内容啦。
